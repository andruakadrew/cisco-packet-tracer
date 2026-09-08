# Inter VLAN routing and troubleshooting
## Overview
In the previous lab I configured a network that had two VLANs between two switches. Devices from one switch were able to communicate with other devices from another switch,
so long as the devices were located in the same VLAN. However, devices in the second VLAN were unable to communicate due to the lack of Inter-VLAN routing. Inter-VLAN routing 
utilizes a layer 3 device in order to route traffic between different VLANs.

In this lab, I configured a network that consist of two VLANs and a default gateway to enable Inter-VLAN routing. After implementation, I purposely misconfigured every network
device in order to examine common networking problems.

## Features
- x4 PCs
- x2 Switches (2960-24TT)
- x1 Router (2901)

## Inter-VLAN Configuration
Before configuring the router, ensure that the switch interface facing the router has trunk mode enabled.

Inside the routers configuration terminal, create subinterfaces.
```
R1(config)#interface g0/0.13
R1(config)#interface g0/0.24
```
Inside every VLAN subinterface, enable encapsulation from 802.1q and include the VLAN ID. Next, add an IP address and subnet mask for the VLAN default gateway.
```
R1(config-subif)#encapsulation dot1q 13
R1(config-subif)#ip add 10.0.0.1 255.255.255.128
```
```
R1(config-subif)#encapsulation dot1q 24
R1(config-subif)#ip add 10.0.0.129 255.255.255.128
```

After the creating the subinterfaces, devices from every VLAN will be able to communicate. Test from PC1 using the ping command. All results should be 75%-100% successful.


## Troubleshooting
Misconfigurations are something that every Network Engineer will experience when troubleshooting problems. A multi network lab with VLAN13 and VLAN24 was experiencing issues
with devices unable to communicate. Using the ping command, all devices were unable to send packets to other devices, or their assigned default gateway.

**Step 1: Switch examination**

Use the the following commands to ensure valid VLAN and trunk assignment
```
SW2#show vlan brief
SW2#show interface trunk
```
After further examination, I found that both switches did not have any trunk ports assigned. Additionally, SW1 had the interface f0/1 assigned to VLAN12. To fix this issue,
move PC1 with interface f0/1 to VLAN13
```
SW1(config-if)#switchport mode access
SW1(config-if)#switchport access vlan 13
```
For both switches, I enabled trunking to ensure vlan communication between switches.
```
SW1(config-if)#switchport mode trunk

```

---

**Step 2: Router examination**

In order for VLANs to communicate outside of their network, a default gateway and 802.1Q needs to be configured. You can check this running the command,
```
R1#show running-config
```
I found that 802.1Q nor default gateway had been configured for the VLANs. To fix this, I created subinterfaces for each VLAN13 and VLAN24
```
interface g0/0.13
encapsulation dot1Q 13
ip address 10.0.0.1 255.255.255.128
```
```
interface g0/0.24
encapsulation dot1Q 24
ip address 10.0.0.129 255.255.255.128
```
After examing the running configuration, I confirmed that both subinterfaces have created and are correct.
