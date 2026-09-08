# Access Control Lists (ACLs) Lab

## Overview

This lab covers the configuration of standard, extended, and named standard Access Control Lists (ACLs) to control traffic flow between network segments. ACLs are used to permit or deny traffic based on criteria like source address, destination address, and protocol. They are evaluated top-down — the router checks each line in order and stops at the first match.

There are two main types covered here:
- **Standard ACLs** (numbered 1–99 or named) filter based on **source IP address only**. Best practice is to place them **close to the destination** to avoid unintentionally blocking traffic.
- **Extended ACLs** (numbered 100–199 or named) filter based on **source IP, destination IP, protocol, and port**. Best practice is to place them **close to the source** to prevent unwanted traffic from crossing the network.

Three exercises are configured using the same base topology, each building on the previous concept.

## Topology

| Device | Interface | IP Address |
|--------|-----------|------------|
| PC1 | NIC | 192.168.1.11/24 |
| PC2 | NIC | 192.168.1.12/24 |
| SW1 | — | (unmanaged) |
| PC3 | NIC | 192.168.2.13/24 |
| PC4 | NIC | 192.168.2.14/24 |
| R1 | F0/0 | 192.168.1.1/24 |
| R1 | F1/0 | 192.168.2.1/24 |
| R1 | S2/0 | 12.0.0.1/24 |
| R2 | S2/0 | 12.0.0.2/24 |
| R2 | F0/0 | 192.168.3.1/24 |
| SRV1 | NIC | 192.168.3.100/24 |
| SRV2 | NIC | 192.168.3.101/24 |

## Exercise 1 — Standard ACLs (Numbered)

### Requirements

1. Only computers in the 192.168.1.0/24 network can access SRV1.
2. PC4 cannot communicate with the 192.168.1.0/24 network.

### Config

#### R2 — Permit only 192.168.1.0/24 to reach SRV1

ACL 1 permits the 192.168.1.0/24 source network. The implicit `deny any` at the end blocks all other sources. Applied outbound on F0/0 so filtered traffic never reaches the 192.168.3.0/24 segment.

```
enable
configure terminal

access-list 1 permit 192.168.1.0 0.0.0.255

interface FastEthernet0/0
 ip access-group 1 out

end
write memory
```

#### R1 — Block PC4 from reaching 192.168.1.0/24

ACL 2 denies PC4's specific host address (192.168.2.14) and then permits everything else with `permit any`. Without the explicit `permit any`, the implicit deny would block all traffic including PC3. Applied outbound on F0/0 so PC4's traffic is dropped before entering the 192.168.1.0/24 network.

```
enable
configure terminal

access-list 2 deny host 192.168.2.14
access-list 2 permit any

interface FastEthernet0/0
 ip access-group 2 out

end
write memory
```

### ACL Placement

| ACL | Router | Interface | Direction | Purpose |
|-----|--------|-----------|-----------|---------|
| 1 | R2 | F0/0 | Out | Permit only 192.168.1.0/24 to SRV1 |
| 2 | R1 | F0/0 | Out | Deny PC4 to 192.168.1.0/24 |

### Verification

```
show access-lists
show ip interface FastEthernet0/0
```

| Test | Expected |
|------|----------|
| PC1 → SRV1 | Success |
| PC2 → SRV1 | Success |
| PC3 → SRV1 | Fail |
| PC4 → 192.168.1.11 | Fail |
| PC3 → 192.168.1.11 | Success |

---

## Exercise 2 — Extended ACLs (Numbered)

### Requirements

1. Only PC1 can access SRV1.
2. Only hosts on the 192.168.2.0/24 network can access SRV2.

### Config

#### R2 — Extended ACL controlling access to both servers

Extended ACL 100 handles both requirements in a single list. The specific permits come first, followed by explicit denies for each server, then a `permit ip any any` at the end so all other traffic flows normally. Order matters — the router evaluates top-down and stops at the first match.

```
enable
configure terminal

access-list 100 permit ip host 192.168.1.11 host 192.168.3.100
access-list 100 permit ip 192.168.2.0 0.0.0.255 host 192.168.3.101
access-list 100 deny ip any host 192.168.3.100
access-list 100 deny ip any host 192.168.3.101
access-list 100 permit ip any any

interface FastEthernet0/0
 ip access-group 100 out

end
write memory
```

Line-by-line breakdown:
- **Line 10** — Permits PC1 (192.168.1.11) to reach SRV1 (192.168.3.100).
- **Line 20** — Permits the entire 192.168.2.0/24 network to reach SRV2 (192.168.3.101). Make sure to use the wildcard mask `0.0.0.255` here — using `host 192.168.2.0` would only match the exact IP 192.168.2.0, which no device has.
- **Line 30** — Denies everyone else to SRV1.
- **Line 40** — Denies everyone else to SRV2.
- **Line 50** — Permits all other traffic so general communication isn't affected.

### ACL Placement

| ACL | Router | Interface | Direction | Purpose |
|-----|--------|-----------|-----------|---------|
| 100 | R2 | F0/0 | Out | Control access to SRV1 and SRV2 |

### Verification

```
show access-lists
show ip interface FastEthernet0/0
```

| Test | Expected |
|------|----------|
| PC1 → SRV1 | Success |
| PC2 → SRV1 | Fail |
| PC3 → SRV2 | Success |
| PC4 → SRV2 | Success |
| PC1 → SRV2 | Fail |
| PC3 → PC1 | Success |

---

## Exercise 3 — Named Standard ACLs

### Requirements

1. Hosts in 192.168.1.0/24 and 192.168.2.0/24 cannot communicate with each other.
2. Hosts in 192.168.2.0/24 cannot access the 192.168.3.0/24 network.

### Config

#### R1 — Block communication between 192.168.1.0/24 and 192.168.2.0/24

Two named ACLs handle each direction. BLOCK-TO-VLAN1 on F0/0 outbound prevents 192.168.2.0/24 from reaching 192.168.1.0/24. BLOCK-TO-VLAN2 on F1/0 outbound prevents 192.168.1.0/24 from reaching 192.168.2.0/24. Both include `permit any` so other traffic like 192.168.3.0/24 can still pass through.

```
enable
configure terminal

ip access-list standard BLOCK-TO-VLAN1
 deny 192.168.2.0 0.0.0.255
 permit any

ip access-list standard BLOCK-TO-VLAN2
 deny 192.168.1.0 0.0.0.255
 permit any

interface FastEthernet0/0
 ip access-group BLOCK-TO-VLAN1 out

interface FastEthernet1/0
 ip access-group BLOCK-TO-VLAN2 out

end
write memory
```

#### R2 — Block 192.168.2.0/24 from reaching the server network

Named ACL BLOCK-VLAN2-TO-SERVERS denies the entire 192.168.2.0/24 network from reaching the 192.168.3.0/24 segment. The `permit any` allows PC1, PC2, and all other traffic to reach the servers.

```
enable
configure terminal

ip access-list standard BLOCK-VLAN2-TO-SERVERS
 deny 192.168.2.0 0.0.0.255
 permit any

interface FastEthernet0/0
 ip access-group BLOCK-VLAN2-TO-SERVERS out

end
write memory
```

### ACL Placement

| ACL | Router | Interface | Direction | Purpose |
|-----|--------|-----------|-----------|---------|
| BLOCK-TO-VLAN1 | R1 | F0/0 | Out | Deny 192.168.2.0/24 to 192.168.1.0/24 |
| BLOCK-TO-VLAN2 | R1 | F1/0 | Out | Deny 192.168.1.0/24 to 192.168.2.0/24 |
| BLOCK-VLAN2-TO-SERVERS | R2 | F0/0 | Out | Deny 192.168.2.0/24 to 192.168.3.0/24 |

### Verification

```
show access-lists
show ip interface FastEthernet0/0
show ip interface FastEthernet1/0
```

| Test | Expected |
|------|----------|
| PC1 → PC3 | Fail |
| PC3 → PC1 | Fail |
| PC3 → SRV1 | Fail |
| PC4 → SRV2 | Fail |
| PC1 → SRV1 | Success |
| PC1 → SRV2 | Success |
