# Static NAT, Dynamic NAT & PAT

A Packet Tracer lab exploring the three forms of Network Address Translation on a topology where a serial link simulates the internet with ACLs that block private addresses.


## Topology

![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/nat-pat-topology.png)

| Device | Interface | IP Address   |
|--------|-----------|--------------|
| R1     | G0/0      | 192.168.1.1  |
| R1     | S0/3/0    | 1.2.3.1      |
| R2     | S0/3/0    | 1.2.3.2      |
| R2     | G0/0      | 1.1.1.1      |
| SRV1   | NIC       | 1.1.1.100    |


## Why PCs Can't Reach SRV1 Without NAT

The serial link between R1 and R2 simulates the internet. An ACL on that link drops any packet sourced from a private address range. Since PC1–PC3 use `192.168.1.0/24`, their traffic is discarded before it reaches R2. NAT solves this by translating private source addresses to public ones at R1.



## Static NAT

One-to-one permanent mapping. Each PC is manually assigned a fixed public address. Used in production for servers or devices that need a consistent public IP.

```
ip nat inside source static 192.168.1.11 1.2.3.11
ip nat inside source static 192.168.1.12 1.2.3.12
ip nat inside source static 192.168.1.13 1.2.3.13

interface g0/0
 ip nat inside
interface s0/3/0
 ip nat outside
```



## Dynamic NAT

A pool of public addresses is defined and assigned on demand. When a host sends traffic, R1 pulls the next available address from the pool. When the session ends, the address returns to the pool. If the pool is exhausted, new sessions are dropped.

```
ip nat pool NATPOOL 1.2.3.10 1.2.3.20 netmask 255.255.255.0
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 pool NATPOOL

interface g0/0
 ip nat inside
interface s0/3/0
 ip nat outside
```



## PAT (NAT Overload)

All inside hosts share a single public IP — R1's S0/3/0 address — differentiated by unique port numbers.

The only difference from dynamic NAT in configuration is replacing the pool reference with the `interface` keyword and adding `overload`:

```
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface s0/3/0 overload

interface g0/0
 ip nat inside
interface s0/3/0
 ip nat outside
```

## Verification

```
R1# show ip nat translations     ! active translation entries
R1# show ip nat statistics       ! hit/miss counts, inside/outside interfaces
```
