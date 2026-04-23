# Layer 2 & Layer 3 EtherChannel Lab

A Packet Tracer lab configuring three EtherChannel bundles across a four-switch topology using PAgP, static mode, and LACP — including a Layer 3 routed port-channel between two multilayer switches.



## Topology

![etherchannel-topology-screenshot](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/etherchannel-topology.JPG)

| Bundle | Switches | Protocol | Type | Interfaces |
|--------|----------|----------|------|------------|
| Po1 | SW1 ↔ SW2 | PAgP | Layer 2 trunk | F0/1, F0/4 |
| Po2 | SW2 ↔ SW3 | Static (mode on) | Layer 3 routed | G0/1, G0/2 |
| Po3 | SW3 ↔ SW4 | LACP | Layer 2 trunk | F0/1, F0/4 |



## Po1 — Layer 2 PAgP (SW1 ↔ SW2)

PAgP is Cisco's proprietary EtherChannel negotiation protocol. `desirable` mode actively negotiates the bundle. The port-channel is configured as a trunk with DTP disabled.

SW2 is a 3560 multilayer switch and requires trunk encapsulation to be set explicitly before the mode can be configured — the 2960 handles this automatically since it only supports dot1q.

```
interface range f0/1, f0/4
 channel-group 1 mode desirable
 no shutdown

interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport nonegotiate
```



## Po2 — Layer 3 Static EtherChannel (SW2 ↔ SW3)

A Layer 3 port-channel routes traffic directly rather than switching it — the bundle gets an IP address instead of being configured as a trunk. `no switchport` must be applied to the physical interfaces first before the port-channel inherits Layer 3 behavior.

Static mode `on` forces the channel up with no negotiation protocol. Both sides must be set to `on` — mixing `on` with `active` or `desirable` will prevent the bundle from forming.

```
interface range g0/1, g0/2
 no switchport
 channel-group 2 mode on
 no shutdown

interface port-channel 2
 ip address 23.0.0.1 255.255.255.252

interface port-channel 2
 ip address 23.0.0.2 255.255.255.252
```



## Po3 — Layer 2 LACP (SW3 ↔ SW4)

LACP is the IEEE 802.3ad open standard equivalent of PAgP. `active` mode actively negotiates — at least one side must be `active` for the bundle to form. SW3 requires trunk encapsulation set explicitly same as SW2.

```
interface range f0/1, f0/4
 channel-group 3 mode active
 no shutdown

interface port-channel 3
 switchport trunk encapsulation dot1q   
 switchport mode trunk
 switchport nonegotiate
```



## Protocol Comparison

| Protocol | Standard | Modes | Both sides on? |
|----------|----------|-------|----------------|
| PAgP | Cisco proprietary | `desirable` / `auto` | At least one `desirable` |
| LACP | IEEE 802.3ad | `active` / `passive` | At least one `active` |
| Static | None | `on` / `on` | Both must be `on` |



## 3560 vs 2960 — Trunk Encapsulation

The 3560 multilayer switch supports both dot1q and ISL encapsulation, so IOS requires you to specify it explicitly before setting trunk mode. Skipping this step produces the error:

```
Command rejected: An interface whose trunk encapsulation is "Auto" can not be configured to "trunk" mode.
```

The 2960 only supports dot1q and sets encapsulation automatically — this command is not needed on 2960s.



## Verification

```
SW1# show etherchannel summary    
SW2# show etherchannel summary 
SW3# show etherchannel summary   
SW4# show etherchannel summary   

SW2# show ip interface brief     
SW3# show ip interface brief      
```

| Flag | Meaning |
|------|---------|
| SU | Layer 2, bundle active |
| RU | Layer 3, bundle active |
| P | Port actively bundled |
| I | Stand-alone — bundle failed |
| s | Suspended — mode mismatch |

Po2 shows `RU` instead of `SU` because it is a routed Layer 3 port-channel rather than a switched trunk.
