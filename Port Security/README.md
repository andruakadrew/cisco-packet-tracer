# Port Security Lab
## Overview
Port security is a Layer 2 security feature on Cisco switches that restricts the number of MAC addresses allowed on a switch port. It helps prevent unauthorized devices from 
connecting and can mitigate MAC address flooding attacks. However, for modern large scale networks, Port security and it's features are considered outdated and there are more 
effecient alternatives. One major flaw of Port Security is the fact that every Shutdown port must be manually brought back up. This is unrealistic for modern enterprises 
because hardware is constantly getting replaced/upgraded, and potentially hundreds of employees to account for.

For this lab, I will configure port security for two access ports and examine how the _mac address table_ stores traffic. Then I will fix a _Secure-Shutdown_ err port.

## Topology
- x2 Switches (2960)
- x2 PCs

## Examine the MAC Address Table
Traffic must be generated, use the ping command from PC1 and ping the PC2 IP address
```
C:\>ping 192.168.1.12

Pinging 192.168.1.12 with 32 bytes of data:

Reply from 192.168.1.12: bytes=32 time<1ms TTL=128
Reply from 192.168.1.12: bytes=32 time=1ms TTL=128
Reply from 192.168.1.12: bytes=32 time=8ms TTL=128
Reply from 192.168.1.12: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.12:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 8ms, Average = 2ms

C:\>
```

### On SW1
```
SW1#show mac address-table
```

A table will print showing the source mac addresses learned from traffic passing through.
```
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.9626.4101    DYNAMIC     Fa0/1
   1    0002.16e2.2193    STATIC      Fa0/2
   1    0007.ec28.0631    DYNAMIC     Fa0/1
```

---

## Configure Port Security
For each switch, enter the Interface facing the end device (e.g f0/2). Run the command
```
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 1
SW1(config-if)# switchport port-security mac-address 00E0.A3xx.xxxx
SW1(config-if)# switchport port-security violation shutdown
SW1(config-if)# end
```

In privledged mode, verify the configurations using the command
```
SW1# show port-security interface f0/2
```

---

## Secure-shutdown
When an unauthorized mac address attempts to communicate through the security port, the port will go into _Secure-shutdown_ mode. While the violation is active, 
No traffic — even from the authorized MAC — can pass while the violation is active. The suspended state must be manually brought back up using the _shutdown_ commands.

Using the ```show port-security interface f0/2``` we can examine the Port Status.

To manually bring the interface back to a working state, move into the interface and run these commands
```
SW2(config-if)#shutdown

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down
SW2(config-if)#no shutdown

SW2(config-if)#
%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to up

```

### Verify communication
Use PC1 and ```ping``` the PC2 IP address. All packets should now be sent and received.



