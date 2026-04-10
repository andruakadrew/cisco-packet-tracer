# DHCP Server, Client & Relay

A Packet Tracer lab exploring centralized DHCP service across a multi-subnet topology using a single DHCP server, a router acting as a DHCP client, and a relay agent bridging subnets.

## Topology

![dhcp-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/dhcp-topology.png)

| Device | Interface | IP Address      |
|--------|-----------|-----------------|
| R1     | G0/0      | 192.168.12.1/24 |
| R1     | G0/1      | 10.0.0.1/24     |
| R2     | G0/0      | DHCP client     |
| R2     | G0/1      | 20.0.0.1/24     |

## DHCP Server (R1)

R1 serves as the central DHCP server for all three networks. Three pools are defined — one for each subnet. Excluded address ranges protect gateway IPs and reserved devices from being handed out dynamically.

```
ip dhcp excluded-address 10.0.0.1 10.0.0.10
ip dhcp excluded-address 20.0.0.1 20.0.0.10

ip dhcp pool 10pool
 network 10.0.0.0 255.255.255.0
 default-router 10.0.0.1
 dns-server 10.0.0.1

ip dhcp pool 20pool
 network 20.0.0.0 255.255.255.0
 default-router 20.0.0.1
 dns-server 20.0.0.1

ip dhcp pool 12pool
 network 192.168.12.0 255.255.255.0
```

The 12pool has no default-router or DNS configured since it serves a point-to-point router link rather than end hosts.

## DHCP Client — R2 G0/0

Rather than a static IP, R2's G0/0 is configured to lease its address directly from R1's 12pool. This simulates how ISP-facing or WAN interfaces are commonly handled in real deployments.

```
interface g0/0
 ip address dhcp
 no shutdown
```

R2 will receive an address in the 192.168.12.0/24 range. Verified with `show dhcp lease` on R2.

## DHCP Relay — R2 G0/1

DHCP relies on Layer 2 broadcasts, which routers do not forward between subnets by default. Without intervention, PC3 and PC4 on the 20.0.0.0/24 network would never reach R1's DHCP server.

The `ip helper-address` command on R2's G0/1 solves this by intercepting DHCP broadcasts from the 20.0.0.0/24 subnet and forwarding them as unicast packets to R1. R1 receives the relayed request, matches it to 20pool based on the source subnet, and returns an address through R2 back to the requesting host.

```
interface g0/1
 ip address 20.0.0.1 255.255.255.0
 ip helper-address 192.168.12.1
 no shutdown
```

## Verification

```
R1# show ip dhcp binding       
R1# show ip dhcp pool          
R1# show ip dhcp conflict     
R2# show dhcp lease            
```
