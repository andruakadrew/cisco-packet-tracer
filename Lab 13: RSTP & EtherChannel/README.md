# RSTP & EtherChannel Lab

A Packet Tracer lab demonstrating Rapid Spanning Tree Protocol (RSTP) and EtherChannel using both LACP and PAgP across a 3-switch topology with a router and end hosts.

---

## Topology Overview

![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/rstp-topology.png)

---

## Device & Address Plan

| Device | Interface     | IP Address       | Role            |
|--------|---------------|------------------|-----------------|
| R1     | G0/0          | 192.168.1.1/24   | Default Gateway |
| SW1    | VLAN 1 SVI    | 192.168.1.2/24   | Root Bridge     |
| SW2    | VLAN 1 SVI    | 192.168.1.3/24   | Access Switch   |
| SW3    | VLAN 1 SVI    | 192.168.1.4/24   | Access Switch   |
| PC15-17  | —             | 192.168.1.10-12  | End Hosts       |
| PC17-20  | —             | 192.168.1.20-22  | End Hosts       |

---

## Cable Layout

| Cable # | From        | To          | Type         | Purpose                  |
|---------|-------------|-------------|--------------|--------------------------|
| 1       | R1 G0/0     | SW1 G0/1    | Straight-through | Router uplink        |
| 2       | SW1 F0/1    | SW2 F0/1    | Crossover    | LACP bundle link 1       |
| 3       | SW1 F0/2    | SW2 F0/2    | Crossover    | LACP bundle link 2       |
| 4       | SW1 F0/3    | SW3 F0/3    | Crossover    | PAgP bundle link 1       |
| 5       | SW1 F0/4    | SW3 F0/4    | Crossover    | PAgP bundle link 2       |
| 6       | SW2 F0/5    | SW3 F0/5    | Crossover    | Redundant (RSTP blocks)  |
| 7-12    | SW2/SW3     | PC1-PC6     | Straight-through | End host connections |

---

## Configuration

### Step 1 — Basic Switch Setup

Repeat on each switch, adjusting hostname and IP accordingly.

**SW1**
```
SW1(config)# hostname SW1
SW1(config)# no ip domain-lookup
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config)# ip default-gateway 192.168.1.1
```

**SW2**
```
SW2(config)# hostname SW2
SW2(config)# no ip domain-lookup
SW2(config)# interface vlan 1
SW2(config-if)# ip address 192.168.1.3 255.255.255.0
SW2(config-if)# no shutdown
SW2(config)# ip default-gateway 192.168.1.1
```

**SW3**
```
SW3(config)# hostname SW3
SW3(config)# no ip domain-lookup
SW3(config)# interface vlan 1
SW3(config-if)# ip address 192.168.1.4 255.255.255.0
SW3(config-if)# no shutdown
SW3(config)# ip default-gateway 192.168.1.1
```

---

### Step 2 — Enable RSTP and Set Root Bridge

Run on all three switches:
```
SW1(config)# spanning-tree mode rapid-pvst
SW2(config)# spanning-tree mode rapid-pvst
SW3(config)# spanning-tree mode rapid-pvst
```

Force SW1 as root bridge by lowering its priority (default is 32768 — lower wins):
```
SW1(config)# spanning-tree vlan 1 priority 4096
```

---

### Step 3 — EtherChannel SW1 ↔ SW2 (LACP)

LACP is an open standard (IEEE 802.3ad). Mode `active` means the switch actively negotiates the bundle.

**SW1:**
```
SW1(config)# interface range f0/1 - 2
SW1(config-if-range)# channel-group 1 mode active
SW1(config-if-range)# no shutdown
SW1(config)# interface port-channel 1
SW1(config-if)# switchport mode trunk
SW1(config-if)# duplex full
```

**SW2:**
```
SW2(config)# interface range f0/1 - 2
SW2(config-if-range)# channel-group 1 mode active
SW2(config-if-range)# no shutdown
SW2(config)# interface port-channel 1
SW2(config-if)# switchport mode trunk
SW2(config-if)# duplex full
```

> **LACP modes:** `active` (negotiates) or `passive` (waits). At least one side must be `active`.

---

### Step 4 — EtherChannel SW1 ↔ SW3 (PAgP)

PAgP is Cisco proprietary. Mode `desirable` means the switch actively negotiates the bundle.

**SW1:**
```
SW1(config)# interface range f0/3 - 4
SW1(config-if-range)# channel-group 2 mode desirable
SW1(config-if-range)# no shutdown
SW1(config)# interface port-channel 2
SW1(config-if)# switchport mode trunk
SW1(config-if)# duplex full
```

**SW3:**
```
SW3(config)# interface range f0/3 - 4
SW3(config-if-range)# channel-group 2 mode desirable
SW3(config-if-range)# no shutdown
SW3(config)# interface port-channel 2
SW3(config-if)# switchport mode trunk
SW3(config-if)# duplex full
```

> **PAgP modes:** `desirable` (negotiates) or `auto` (waits). At least one side must be `desirable`.

---

### Step 5 — Redundant Link SW2 ↔ SW3

Connect F0/5 on both switches and configure as trunk. RSTP will automatically block one end.

```
SW2(config)# interface f0/5
SW2(config-if)# switchport mode trunk

SW3(config)# interface f0/5
SW3(config-if)# switchport mode trunk
```

---

### Step 6 — Router R1

```
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
```

---

## Verification

### Spanning Tree
```
SW1# show spanning-tree
```

Expected output highlights:
- `Spanning tree enabled protocol rstp` — confirms RSTP is running
- `This bridge is the root` — confirms SW1 won the election
- All SW1 ports show `Desg FWD`
- Po1 and Po2 show `P2p` (if duplex full is set)

```
SW2# show spanning-tree
SW3# show spanning-tree
```

On SW2 or SW3, F0/5 should show `Altn BLK` — this is RSTP blocking the redundant link.

### EtherChannel
```
SW1# show etherchannel summary
```

Expected output:
```
Group  Port-channel  Protocol  Ports
1      Po1(SU)       LACP      Fa0/1(P) Fa0/2(P)
2      Po2(SU)       PAgP      Fa0/3(P) Fa0/4(P)
```

| Flag | Meaning                        |
|------|--------------------------------|
| S    | Layer 2 port-channel           |
| U    | Bundle is in use               |
| P    | Port is actively bundled       |
| I    | Stand-alone (bundle failed)    |
| s    | Suspended (mode mismatch)      |

```
SW1# show lacp neighbor      ! verify LACP on Po1
SW1# show pagp neighbor      ! verify PAgP on Po2
```

---

## Concepts Demonstrated

### RSTP (Rapid Spanning Tree Protocol — IEEE 802.1w)
RSTP prevents Layer 2 loops by placing redundant ports into a blocking state. Unlike legacy STP which takes 30-50 seconds to reconverge, RSTP converges in 1-2 seconds using port roles and direct negotiation between switches.

| Port Role  | State      | Description                              |
|------------|------------|------------------------------------------|
| Root       | Forwarding | Best path toward the root bridge         |
| Designated | Forwarding | Best port on each network segment        |
| Alternate  | Blocking   | Backup path to root (replaces Blocking)  |
| Backup     | Blocking   | Redundant port on same segment           |

### EtherChannel
EtherChannel bundles multiple physical links into one logical interface, providing both redundancy and increased bandwidth. If one member link fails, traffic continues over the remaining links without STP reconvergence.

| Protocol | Standard          | Modes                    |
|----------|-------------------|--------------------------|
| LACP     | IEEE 802.3ad (open) | `active` / `passive`   |
| PAgP     | Cisco proprietary | `desirable` / `auto`     |

### Why the SW2 F0/5 Port is Orange in Packet Tracer
Orange means the port is in RSTP **blocking** state — this is correct and intentional. RSTP detected a redundant path between SW2 and SW3 and blocked one end to prevent a loop. The port is still listening in the background and will unblock within seconds if the primary path fails.

---

## Failover Test

Simulate a link failure to observe RSTP rapid convergence:

```
SW1(config)# interface port-channel 1
SW1(config-if)# shutdown
```

Watch SW2's F0/5 flip from orange (blocking) to green (forwarding) within 1-2 seconds. Restore with:

```
SW1(config-if)# no shutdown
```

---

## NTP Configuration (Bonus — configured on R1)

```
R1# clock timezone EST -5
R1# clock summer-time EDT recurring
R1# clock set 10:30:00 07 Apr 2026
R1(config)# ntp master
```

R2 and R3 sync to R1:
```
R2(config)# ntp server 192.168.12.1
R3(config)# ntp server 192.168.23.2
```

Verify:
```
R1# show ntp status
R2# show ntp associations
```
