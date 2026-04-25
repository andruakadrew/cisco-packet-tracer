# Inter-VLAN Routing — SVI & Router-on-a-Stick

A Packet Tracer lab implementing two inter-VLAN routing methods side by side — SVIs on a Layer 3 switch for VLAN10 and VLAN20, and router-on-a-stick subinterfaces on R2 for VLAN30 and VLAN40.



## Topology

![inter-vlan-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/intervlan-topology.png)

| Device | VLAN | IP Address   | Gateway    |
|--------|------|--------------|------------|
| PC1    | 10   | 10.0.1.10/24 | 10.0.1.1   |
| PC2    | 20   | 10.0.2.10/24 | 10.0.2.1   |
| PC3    | 30   | 10.0.3.10/24 | 10.0.3.1   |
| PC4    | 40   | 10.0.4.10/24 | 10.0.4.1   |



## Method 1 — SVI Inter-VLAN Routing (SW1)

SW1 is a 3560 multilayer switch capable of routing between VLANs internally using Switch Virtual Interfaces (SVIs). Each VLAN is assigned an IP address directly on the switch which acts as the default gateway for hosts in that VLAN.

`ip routing` must be explicitly enabled on the 3560 — it is off by default and without it the SVIs come up but traffic will not be routed between VLANs.

```
ip routing

vlan 10
vlan 20

interface vlan 10
 ip address 10.0.1.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 10.0.2.1 255.255.255.0
 no shutdown

interface f0/1
 switchport mode access
 switchport access vlan 10

interface f0/2
 switchport mode access
 switchport access vlan 20
```



## Method 2 — Router-on-a-Stick (R2 & SW2)

SW2 is a 2960 Layer 2 only switch — it cannot route between VLANs on its own. R2 performs inter-VLAN routing via subinterfaces on a single physical trunk link. Each subinterface is tagged with a VLAN ID using `encapsulation dot1q` and assigned the gateway IP for that VLAN.

SW2's G0/1 facing R2 must be configured as a trunk to carry both VLAN30 and VLAN40 tagged traffic on the same physical link.

```
! SW2
vlan 30
vlan 40

interface f0/1
 switchport mode access
 switchport access vlan 30

interface f0/2
 switchport mode access
 switchport access vlan 40

interface g0/1
 switchport mode trunk
 switchport nonegotiate
 no shutdown
```

```
! R2
interface g0/1
 no shutdown

interface g0/1.30
 encapsulation dot1q 30
 ip address 10.0.3.1 255.255.255.0

interface g0/1.40
 encapsulation dot1q 40
 ip address 10.0.4.1 255.255.255.0
```



## SVI vs Router-on-a-Stick

| | SVI (Layer 3 Switch) | Router-on-a-Stick |
|---|---|---|
| Routing location | Switch itself | External router |
| Hardware required | Multilayer switch | Any router + L2 switch |
| Performance | Higher — no external link bottleneck | Limited by single trunk link bandwidth |
| Scalability | Better for large VLAN counts | Single link becomes bottleneck with many VLANs |
| Key command | `ip routing` | `encapsulation dot1q` |



## Verification

```
SW1# show ip route                     ! confirm directly connected routes for VLAN10/20
SW1# show interfaces vlan 10           ! confirm SVI is up/up
SW1# show interfaces vlan 20           ! confirm SVI is up/up
SW2# show interfaces trunk             ! confirm G0/1 is trunking
R2# show ip interface brief            ! confirm subinterfaces are up/up
```
