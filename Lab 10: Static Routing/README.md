# Static Routing Lab

## Overview

This lab covers the configuration of static routes across two routers and the implementation of a floating static route as a backup path. Static routes are manually configured paths that tell a router how to reach a remote network. A floating static route is a backup route that only becomes active when the primary route fails, making it a simple and effective redundancy solution.

---

## Topology

<img width="368" height="80" alt="image" src="https://github.com/user-attachments/assets/c2f2e646-21a4-4a0a-8cc4-61c5f75e5b9a" />


| Device | Interface | IP Address      |
|--------|-----------|-----------------|
| PC1    | NIC       | 192.168.1.11/24 |
| SW1    | —         | (unmanaged)     |
| R1     | G0/1      | 192.168.1.1/24  |
| R1     | G0/0      | 10.0.0.1/24     |
| R2     | G0/0      | 10.0.0.2/24     |
| R2     | G0/1      | 192.168.2.1/24  |
| SW2    | —         | (unmanaged)     |
| PC2    | NIC       | 192.168.2.12/24 |

---

## Static Routing Config

Each router needs a static route for every network it is not directly connected to. The next-hop IP is the neighboring router's address on their shared link.

### R1

```
enable
configure terminal

ip route 192.168.2.0 255.255.255.0 10.0.0.2

end
write memory
```

### R2

```
enable
configure terminal

ip route 192.168.1.0 255.255.255.0 10.0.0.1

end
write memory
```

### Verify

```
show ip route
```

Look for `S` entries confirming each static route is active. Then test end-to-end connectivity from PC1:

```
ping 192.168.2.12
tracert 192.168.2.12
```

---

## Floating Static Route Config

A floating static route serves as a backup path. It is assigned a higher administrative distance (AD) than the primary route so the router ignores it unless the primary route goes down.

> **Note:** RIP has a default AD of 120. Setting the floating static to AD 121 ensures it only activates when the primary RIP-learned route is lost.

### On R1 — backup route to PC2's subnet with elevated AD

```
enable
configure terminal

ip route 192.168.2.0 255.255.255.0 10.0.0.2 121

end
write memory
```

The `121` at the end is the administrative distance that makes this route float behind the primary.

### Verify

Under normal operation the floating static will not appear in `show ip route` — the primary route takes precedence. To confirm the backup works, simulate a failure by shutting down R1's primary interface:

```
interface GigabitEthernet0/0
 shutdown
```

Run `show ip route` again — the floating static should now appear as:

```
S    192.168.2.0/24 [121/0] via 10.0.0.2
```

Restore the interface when done:

```
interface GigabitEthernet0/0
 no shutdown
```
