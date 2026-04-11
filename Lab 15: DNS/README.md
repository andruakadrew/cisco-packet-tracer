# DHCP & DNS Resolution Lab

A Packet Tracer lab exploring the relationship between DHCP, DNS, and name resolution across a multi-subnet topology. The lab demonstrates how DNS server assignment via DHCP enables hostname resolution for end hosts, and where the boundaries of that resolution lie for network devices.


## Topology

![dns-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/dns-topology.png)

| Device | IP Address    | Role             |
|--------|---------------|------------------|
| R1     | 192.168.1.1 / 10.0.0.1 / 20.0.0.1 | Gateway, DHCP server |
| DNS1   | 20.0.0.100    | DNS server       |
| SW1    | 192.168.1.2   | Access switch    |
| SW2    | 10.0.0.2      | Access switch    |
| SRV1   | 10.0.0.101    | Server           |
| SRV2   | 10.0.0.102    | Server           |


## DHCP Pool (R1)

R1 serves the 192.168.1.0/24 network with a single pool. The DNS server is intentionally omitted in the initial configuration to demonstrate the difference between IP reachability and name resolution.

```
ip dhcp excluded-address 192.168.1.1 192.168.1.10

ip dhcp pool 1pool
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
```

Once DNS is added to the pool, PC1 and PC2 need to renew their leases to receive the updated configuration:

```
ip dhcp pool 1pool
 dns-server 20.0.0.100
```



## DNS Configuration (DNS1)

DNS1 runs Packet Tracer's built-in DNS service with A records for all devices in the topology. Key records relevant to this lab:

| Name | Type     | Address      |
|------|----------|--------------|
| srv1 | A Record | 10.0.0.101   |
| srv2 | A Record | 10.0.0.102   |
| dns1 | A Record | 20.0.0.100   |
| r1   | A Record | 192.168.1.1  |



## IP Reachability vs Name Resolution

A core observation in this lab is that IP reachability and name resolution are independent. Before DNS is added to the DHCP pool, PC1 can ping SRV1 by IP (`10.0.0.101`) successfully but fails to ping by name (`srv1`) because no DNS server has been assigned. Once the dns-server line is added to 1pool and PC1 renews its lease, both pings succeed.



## Switch DNS Limitations

SW1 is configured with a default gateway and name-server manually since it does not participate in DHCP:

```
SW1(config)# ip default-gateway 192.168.1.1
SW1(config)# ip name-server 20.0.0.100
```

SW1 can reach DNS1 at 20.0.0.100 and has the correct name-server configured, but fails to resolve hostnames from the CLI. This is a known Packet Tracer limitation — Layer 2 switches in PT do not fully support DNS client functionality. On a real IOS switch this configuration resolves hostnames correctly.



## Verification

```
R1# show ip dhcp binding        ! confirm PC1/PC2 leased addresses with DNS assigned
PC1> ipconfig /all              ! confirm dns-server 20.0.0.100 appears after lease renewal
PC1> ping srv1                  ! confirms end-to-end DNS resolution and routing
PC1> ping srv2                  ! confirms resolution across subnets
SW1# ping 20.0.0.100            ! confirms SW1 can reach DNS server
```
