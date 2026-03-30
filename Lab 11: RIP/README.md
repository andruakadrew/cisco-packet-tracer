# RIPv2 Lab

## Overview

This lab covers the configuration of RIP version 2 (RIPv2) across four routers to achieve full network connectivity. RIP (Routing Information Protocol) is a distance-vector routing protocol that uses hop count as its metric to determine the best path to a destination. RIPv2 improves on version 1 by supporting classless routing and including subnet mask information in its updates. Passive interfaces are also configured on each router to stop RIP updates from being sent out toward switches and end devices.

---

## Topology

<img width="851" height="444" alt="Screenshot 2026-03-30 115535" src="https://github.com/user-attachments/assets/95bf71b7-f5ff-46b9-9f12-a323e085d71f" />

| Device | Interface | IP Address    |
|--------|-----------|---------------|
| PC1    | NIC       | 10.0.0.10/24  |
| SW1    | —         | (unmanaged)   |
| R1     | G0/2      | 10.0.0.1/24   |
| R1     | G0/0      | 12.0.0.1/24   |
| R1     | G0/1      | 13.0.0.1/24   |
| R2     | G0/0      | 12.0.0.2/24   |
| R2     | G0/1      | 24.0.0.2/24   |
| R2     | G0/2      | 20.0.0.1/24   |
| SW2    | —         | (unmanaged)   |
| PC2    | NIC       | 20.0.0.10/24  |
| R3     | G0/1      | 13.0.0.3/24   |
| R3     | G0/0      | 34.0.0.3/24   |
| R3     | G0/2      | 30.0.0.1/24   |
| SW3    | —         | (unmanaged)   |
| PC3    | NIC       | 30.0.0.10/24  |
| R4     | G0/0      | 34.0.0.4/24   |
| R4     | G0/1      | 24.0.0.4/24   |
| R4     | G0/2      | 40.0.0.1/24   |
| SW4    | —         | (unmanaged)   |
| PC4    | NIC       | 40.0.0.10/24  |

---

## RIPv2 Config

RIPv2 is configured on all four routers. Each router advertises every network connected to its interfaces. `no auto-summary` is required to prevent RIP from summarizing routes back to their classful boundary, ensuring specific subnets are advertised correctly.

### R1

```
enable
configure terminal

router rip
 version 2
 no auto-summary
 network 10.0.0.0
 network 12.0.0.0
 network 13.0.0.0
 passive-interface GigabitEthernet0/2

end
write memory
```

### R2

```
enable
configure terminal

router rip
 version 2
 no auto-summary
 network 12.0.0.0
 network 24.0.0.0
 network 20.0.0.0
 passive-interface GigabitEthernet0/2

end
write memory
```

### R3

```
enable
configure terminal

router rip
 version 2
 no auto-summary
 network 13.0.0.0
 network 34.0.0.0
 network 30.0.0.0
 passive-interface GigabitEthernet0/2

end
write memory
```

### R4

```
enable
configure terminal

router rip
 version 2
 no auto-summary
 network 34.0.0.0
 network 24.0.0.0
 network 40.0.0.0
 passive-interface GigabitEthernet0/2

end
write memory
```

---

## Passive Interface Config

Passive interfaces stop RIP from sending routing updates out of interfaces that face switches and end devices. RIP will still advertise those networks to other routers — it simply won't waste bandwidth sending updates to devices that don't need them.

> The `passive-interface` command is already included in each router's config above. The table below summarizes which interface is passive on each router and why.

| Router | Passive Interface | Connected To |
|--------|-------------------|--------------|
| R1     | G0/2              | SW1 (PC1)    |
| R2     | G0/2              | SW2 (PC2)    |
| R3     | G0/2              | SW3 (PC3)    |
| R4     | G0/2              | SW4 (PC4)    |

---

## Verification

### Confirm RIPv2 is running and passive interfaces are set

```
show ip protocols
```

Look for `version 2`, the list of advertised networks, and passive interfaces listed under `Passive Interface(s)`.

### Check the routing table

```
show ip route
```

You should see `R` entries for all remote subnets. Example output on R1:

```
R    20.0.0.0/24 [120/1] via 12.0.0.2
R    24.0.0.0/24 [120/1] via 12.0.0.2
R    30.0.0.0/24 [120/1] via 13.0.0.3
R    34.0.0.0/24 [120/1] via 13.0.0.3
R    40.0.0.0/24 [120/2] via 12.0.0.2
```

### Test end-to-end connectivity from PC1

```
ping 40.0.0.10
tracert 40.0.0.10
```

A successful ping from PC1 to PC4 confirms full connectivity across all four routers.
