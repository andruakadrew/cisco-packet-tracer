# VLAN Configuration and Trunk Encapsulation lab
## Overview
Virtual Local Area Networks (VLANs) provide logical segmentation of a physical network by creating separate Layer 2 broadcast domains. Devices within the same VLAN can 
communicate with each other regardless of their physical location.

To allow VLAN traffic to travel between switches, trunk links are configured using the IEEE 802.1Q standard. Trunking enables multiple VLANs to share a single physical link 
by tagging Ethernet frames with VLAN identifiers.

In this lab, I configured VLANs and a trunk link between two Cisco switches in Packet Tracer. Then I assigned two end devices to VLAN2 while keeping the other end device in
default VLAN1. 

## Features
- x2 3650-24PS Multicast switches
- x3 PCs

## Steps
1. **Step 1:** Ping all three PCs. Communication will be 100% successful because no VLANs have been created yet.
2. **Step 2:** For both switches, enter _Configuration mode_ and create VLAN2.
```
SW1(config)#vlan 2
```
3. **Step 3:** While in the interface for VLAN2, name it accordingly (e.g. VLAN2, VLAN-2)
```
SW1(config-vlan)#name VLAN2
```
4. **Step 4:** Move two devices from default VLAN1 to VLAN2. Leave one device inside default VLAN1
```
SW2(config)#int f0/2
SW2(config-if)#sw mode access
SW2(config-if)#sw access vlan 2
```
```
SW1(config)#int f0/3
SW1(config-if)#sw mode access
SW1(config-if)#sw access vlan 2
```
5. **Step 5:** Enable _trunk port_ for both switch interfaces  (f0/1 - f0/1)
```
SW2(config)#int f0/1
SW2(config-if)#sw mode access
SW2(config-if)#sw mode trunk
```
6. **Step 6:** Test with ping again, both PCs on VLAN2 are able to communicate with eachother but the PC on deafult VLAN1 cannot be reached because it belongs to a different
VLAN.
