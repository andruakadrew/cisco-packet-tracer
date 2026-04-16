# RIP, Syslog, PAT, DHCP & SSH Lab

A Packet Tracer lab combining five core network services into a single multi-router topology — RIPv2 for dynamic routing, PAT for address translation, centralized DHCP with relay, syslog for log aggregation, and SSH for secure remote access.



## Topology

![config-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/configuration-topology.JPG)

| Device | Interface | IP Address    |
|--------|-----------|---------------|
| R1     | G0/0      | 192.168.1.1   |
| R1     | G0/1      | 1.2.3.1       |
| R2     | G0/0      | 192.168.2.1   |
| R2     | G0/1      | 1.2.3.2       |
| R3     | G0/1      | 1.2.3.3       |
| R3     | G0/0      | 30.0.0.1      |
| SRV1   | NIC       | 30.0.0.100    |



## RIPv2

RIPv2 is configured on all three routers to advertise their directly connected networks. `no auto-summary` is required to prevent RIP from summarizing to classful boundaries, which would cause incorrect routing with the discontiguous subnets in this topology.

```
router rip
 version 2
 no auto-summary
 network 192.168.1.0
 network 1.2.3.0
```

Each router advertises only its own connected networks. R1 learns about `192.168.2.0/24` and `30.0.0.0/24` via RIP updates from R2 and R3.



## Syslog

All three routers send log messages to SRV1 at `30.0.0.100`. This centralizes event logging so interface state changes, errors, and config events are captured in one place rather than lost on individual console buffers.

```
logging host 30.0.0.100
```

Configured identically on R1, R2, and R3. Verified under **SRV1 → Services → Syslog** after triggering interface events.



## PAT (NAT Overload)

R1 and R2 each translate their inside hosts to their own G0/1 interface address using PAT. The `overload` keyword allows all inside hosts to share a single public IP differentiated by port numbers.

```
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface g0/1 overload

interface g0/0
 ip nat inside
interface g0/1
 ip nat outside
```

R2 uses an identical config substituting its own `192.168.2.0/24` ACL. Using the `interface` keyword instead of a static pool means the translation automatically uses whatever IP is assigned to G0/1 — no reconfiguration needed if the address changes.



## DHCP Server & Relay (R1)

R1 serves as the central DHCP server for both LAN segments. PC4-6 on R2's network are served via a relay agent since their DHCP broadcasts can't cross the routed boundary to R1 on their own.

```
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp excluded-address 192.168.2.1 192.168.2.10

ip dhcp pool 1pool
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 30.0.0.100

ip dhcp pool 2pool
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 30.0.0.100
```

R2's G0/0 interface forwards DHCP broadcasts from the `192.168.2.0/24` subnet to R1:

```
interface g0/0
 ip helper-address 1.2.3.1
```



## SSH Version 2 (R1)

SSH is configured on R1 for secure remote access. A domain name and RSA key pair are required before SSHv2 can be enabled. All VTY lines are restricted to SSH only

```
ip domain-name cisco.com
crypto key generate rsa        ! enter 1024 when prompted
ip ssh version 2
username cisco secret ccna

line vty 0 15
 login local
 transport input ssh
```

Configuring `line vty 0 15` as a single block ensures no VTY lines fall back to Telnet. Securing only 0-4 leaves lines 5-15 as a potential bypass since connections roll over to higher lines when 0-4 are occupied.



## Verification

```
R1# show ip route rip              ! confirm all subnets learned via RIP
R1# show ip nat translations       ! confirm PAT sessions active after pinging SRV1
R1# show ip dhcp binding           ! confirm all PCs received addresses
R1# show ip ssh                    ! confirm SSH version 2 is running
R2# show run | include helper      ! confirm relay agent pointing to 1.2.3.1
```
