# EIGRP Lab — Unequal-Cost Load Balancing & Route Summarization

A Packet Tracer lab configuring EIGRP AS 100 across a five-router topology, exploring EIGRP's composite metric, unequal-cost load balancing via variance, and manual route summarization on an interface toward a downstream neighbor.



## Topology

![eigrp-topology-screenshot](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/eigrp-topology.png)

| Device | Interface | Network         |
|--------|-----------|-----------------|
| R1     | F1/0      | 10.12.0.0/24    |
| R1     | G0/0      | 10.14.0.0/24    |
| R2     | F1/0      | 10.12.0.0/24    |
| R2     | F2/0      | 10.23.0.0/24    |
| R3     | F1/0      | 10.23.0.0/24    |
| R3     | F2/0      | 10.34.0.0/24    |
| R3     | G0/0      | 10.35.0.0/24    |
| R4     | G0/0      | 10.14.0.0/24    |
| R4     | F1/0      | 10.34.0.0/24    |
| R5     | G0/0      | 10.35.0.0/24    |

Loopbacks: R1=1.1.1.1, R2=2.2.2.2, R3=3.3.3.3, R4=4.4.4.4, R5=5.5.5.5 — all /32



## EIGRP Configuration

EIGRP uses a composite metric based on bandwidth and delay rather than hop count. All routers run AS 100 — the AS number must match between neighbors or adjacencies will never form. `no auto-summary` prevents classful summarization which would cause incorrect routing with discontiguous subnets. Loopback interfaces are set passive since they have no neighbors but still need to be advertised.

Network statements use exact wildcard masks (`0.0.0.0`) to match only the specific interface IP rather than an entire classful range.

```
! R1
router eigrp 100
 no auto-summary
 passive-interface loopback 0
 network 1.1.1.1 0.0.0.0
 network 10.12.0.1 0.0.0.0
 network 10.14.0.1 0.0.0.0

! R2
router eigrp 100
 no auto-summary
 passive-interface loopback 0
 network 2.2.2.2 0.0.0.0
 network 10.12.0.2 0.0.0.0
 network 10.23.0.2 0.0.0.0

! R3
router eigrp 100
 no auto-summary
 passive-interface loopback 0
 network 3.3.3.3 0.0.0.0
 network 10.23.0.3 0.0.0.0
 network 10.34.0.3 0.0.0.0
 network 10.35.0.3 0.0.0.0

! R4
router eigrp 100
 no auto-summary
 passive-interface loopback 0
 network 4.4.4.4 0.0.0.0
 network 10.14.0.4 0.0.0.0
 network 10.34.0.4 0.0.0.0

! R5
router eigrp 100
 no auto-summary
 passive-interface loopback 0
 network 5.5.5.5 0.0.0.0
 network 10.35.0.5 0.0.0.0
```



## Unequal-Cost Load Balancing (R1)

EIGRP is the only IGP that supports unequal-cost load balancing. The `variance` command allows EIGRP to use any feasible successor route whose metric is within `variance × feasible distance` of the best route. A feasible successor must have a reported distance less than the current feasible distance.

R1 has two paths to R5 — via R2 (FastEthernet) and via R4 (GigabitEthernet). The GigabitEthernet path has a lower cost, making it the successor. Setting variance allows R1 to also use the R2 path for load balancing:

```
R1(config)# router eigrp 100
R1(config-router)# variance 2
```

Verify both paths appear in the routing table:
```
R1# show ip route 5.5.5.5
```



## Route Summarization (R3 → R5)

Rather than advertising individual /24 prefixes to R5, R3 summarizes all 10.x.x.x networks into a single `10.0.0.0/8` advertisement. The summary is applied on the **interface facing R5** — EIGRP summarization is configured per-interface.

R3 automatically installs a `Null0` route for `10.0.0.0/8` to prevent routing loops when the summary is active.

```
R3(config)# interface g0/0
R3(config-if)# ip summary-address eigrp 100 10.0.0.0 255.0.0.0
```

R5 receives a single `D 10.0.0.0/8` route instead of multiple specific prefixes.



## Verification

```
R1# show ip eigrp neighbors          
R1# show ip eigrp topology           
R1# show ip route 5.5.5.5            
R3# show ip eigrp interfaces detail g0/0  
R5# show ip route eigrp             
```
