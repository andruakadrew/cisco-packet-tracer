 # Cisco Discovery Protocol
 ## Overview
 CDP allows devices to be able to share information such as,
 - device name
 - model
 - interface connections
 - IOS

In this lab, I will examine multiple types of Cisco hardware and use the CDP
commands to inspect neighboring device information.

## Steps
**1. Identify interfaces berween devices**

Starting with the Router (R1), run the commands:
```
enable
show cdp neighbors
```

### Output from R1
```
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R2           Gig 0/0          157            R       C2900       Gig 0/0
SW1          Gig 0/1          157            S       2960        Gig 0/1
```

---

**2. Identify device models**

To examine neighboring devide models, run the command from any device:
```
show cdp neighbors detail
```

### Output from R2
```
Device ID: SW2
Entry address(es): 
Platform: cisco 3560, Capabilities: 
Interface: GigabitEthernet0/1, Port ID (outgoing port): GigabitEthernet0/1
Holdtime: 169

Version :
Cisco IOS Software, C3560 Software (C3560-ADVIPSERVICESK9-M), Version 12.2(37)SE1, RELEASE SOFTWARE (fc1)
Copyright (c) 1986-2007 by Cisco Systems, Inc.
Compiled Thu 05-Jul-07 22:22 by pt_team

advertisement version: 2
Duplex: full
---------------------------

Device ID: R1
Entry address(es): 
Platform: cisco C1900, Capabilities: Router
Interface: GigabitEthernet0/0, Port ID (outgoing port): GigabitEthernet0/0
Holdtime: 168

Version :
Cisco IOS Software, C1900 Software (C1900-UNIVERSALK9-M), Version 15.1(4)M4, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2012 by Cisco Systems, Inc.
Compiled Thurs 5-Jan-12 15:41 by pt_team

advertisement version: 2
Duplex: full
```

From the output we get
- SW2 model --> ``` Platform: cisco 3560, Capabilities: ```
- R1 model --> ``` Platform: cisco C1900, Capabilities: Router ```

---

**3. Identify IOS version**

To view neighboring device IOS versions, run the command:
```
show cdp neighbors detail
```

### Output from SW2
```
Device ID: R2
Entry address(es): 
Platform: cisco C2900, Capabilities: Router
Interface: GigabitEthernet0/1, Port ID (outgoing port): GigabitEthernet0/1
Holdtime: 169

Version :
Cisco IOS Software, C2900 Software (C2900-UNIVERSALK9-M), Version 15.1(4)M4, RELEASE SOFTWARE (fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2012 by Cisco Systems, Inc.
Compiled Thurs 5-Jan-12 15:41 by pt_team

advertisement version: 2
Duplex: full
```

From the output we get
- R2 IOS version --> ``` Cisco IOS Software, C2900 Software (C2900-UNIVERSALK9-M), Version 15.1(4)M4, RELEASE SOFTWARE (fc2) ```
