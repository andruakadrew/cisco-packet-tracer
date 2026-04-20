# STP & RPVST Lab

A Packet Tracer lab exploring Spanning Tree Protocol election logic, RPVST configuration, per-VLAN root bridge assignment, and PortFast/BPDUGuard on a 3-switch topology with three VLANs.



## Topology

![rpvst-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/stp-topology.JPG)

| Switch | MAC Address    | Default Priority |
|--------|----------------|-----------------|
| SW1    | 0001.4258.19EA | 32769           |
| SW2    | 00D0.97E6.E169 | 32769           |
| SW3    | 00E0.8FA4.BA54 | 32769           |

VLANs: 10 (blue), 20 (purple), 30 (green)



## STP Election Logic

With all switches at the same default priority of 32769, the root bridge is determined by the lowest MAC address — SW1 wins. This is normal 802.1D behavior and the default STP version running on Cisco switches before any configuration changes.

Root port selection follows lowest cost path to root. FastEthernet links carry a cost of 19 and GigabitEthernet a cost of 4. Any port that is neither a root port nor a designated port is placed into blocking state to prevent loops. The blocked port is always on the switch furthest from the root with the highest cost redundant path.



## RPVST

All switches are changed from classic 802.1D STP to RPVST (Rapid Per-VLAN Spanning Tree), which is Cisco's per-VLAN implementation of 802.1w. Convergence drops from 30-50 seconds to 1-2 seconds.

```
spanning-tree mode rapid-pvst
```

Configured identically on SW1, SW2, and SW3.



## Per-VLAN Root Bridge Assignment

Rather than letting one switch become root for all VLANs, root bridges are distributed across switches. This causes each VLAN's traffic to take a different primary path through the network — effectively load balancing at the STP layer.

```
SW1(config)# spanning-tree vlan 10 root primary
SW1(config)# spanning-tree vlan 30 root secondary

SW2(config)# spanning-tree vlan 20 root primary
SW2(config)# spanning-tree vlan 10 root secondary

SW3(config)# spanning-tree vlan 30 root primary
SW3(config)# spanning-tree vlan 20 root secondary
```

The `root primary` macro sets priority to 24576 and `root secondary` sets it to 28672 — both lower than the default 32768 to guarantee election wins regardless of MAC address.

| VLAN | Primary Root | Secondary Root |
|------|-------------|----------------|
| 10   | SW1         | SW2            |
| 20   | SW2         | SW3            |
| 30   | SW3         | SW1            |



## PortFast & BPDUGuard

PortFast bypasses STP's listening and learning states on host-facing ports, putting them directly into forwarding. This eliminates the 30-second delay end devices would otherwise experience when connecting. BPDUGuard protects these ports by immediately shutting them down if a BPDU is received — preventing someone from accidentally introducing a switch on a PortFast port and causing a topology change.

Applied only to access ports facing end hosts:

```
interface range f0/1 - 3
 spanning-tree portfast
 spanning-tree bpduguard enable
```

Configured on the host-facing FastEthernet ports of SW1, SW2, and SW3.



## Verification

```
SW1# show spanning-tree vlan 10     ! confirm SW1 is root, check port roles
SW2# show spanning-tree vlan 20     ! confirm SW2 is root
SW3# show spanning-tree vlan 30     ! confirm SW3 is root
SW1# show spanning-tree interface f0/1 portfast    ! confirm PortFast active
```
