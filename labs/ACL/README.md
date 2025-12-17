# ACL (Access Control List) Configuration lab
In this lab, I configured a Multi Router network to simulate a school environment. This lab contains students, teachers, and a centralized Server network. The goal of this lab is to get a deeper understanding of ACLs
and how I can use it to restrict users based on permissions.


## Lab setup
- **3 Routers**: Router0, Router1, Router2
- **2 Switches** Switch 1(Students), Switch2 (Teachers)
- End Devices
    - 2 Laptops
    - 2 PCs
    - 1 Server


## IP Addressing
**Students Network**
- Network: _10.0.0.0/8_
- Router1 Gateway: _10.0.0.1_
- Laptop0: _10.0.0.10_
- Laptop1: _10.0.0.11_


**Teachers Network**
- Network: _20.0.0.0/8_
- Router2 Gateway: _20.0.0.1_
- PC0: _20.0.0.10_
- PC1: _20.0.0.11_


**Server Network**
- Network:_ 30.0.0.0/8_
- Router0 Gateway: _30.0.0.1_
- Server0: _30.0.0.10_
