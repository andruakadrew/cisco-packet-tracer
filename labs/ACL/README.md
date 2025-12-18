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


![acl-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/ACL/images/acl%20topology.png)


## ACL Design
1. Students **ARE** allowed to reach their own default gateway
2. Students **ARE NOT** allowed to access the Server network
3. Students **ARE NOT** allowed to access the Teachers network
4. Teachers **ARE** allowed to access the Server network
5. All other Students traffic is permitted


## ACL Configuration
### ACL Name
```
STUDENTS_OUT
```


### ACL Rules
```
10 permit ip 10.0.0.0 0.0.0.255 host 10.0.0.1
20 deny ip 10.0.0.0 0.0.0.255 30.0.0.0 0.255.255.255
30 deny ip 10.0.0.0 0.0.0.255 20.0.0.0 0.0.0.255
40 permit ip 10.0.0.0 0.0.0.255 any
```


Interface Application
```
interface GigabitEthernet0/0
ip address 10.0.0.1 255.255.255.0
ip access-group STUDENTS_OUT in
```


## Verification Steps
### ACL Verification
```
show access-list
```

![access-lists](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/ACL/images/show%20access-lists.png)


### Interface Verification
```
show ip interface g0/0
```


![ip-interface](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/ACL/images/show%20ip%20interface.png)


### Ping Verification
On Student Laptop0, the ping commmand was used to confirm the Teacher and Server networks are unreachable. I also used ping to confirm connectivity for the Students default gateway.


```
ping 20.0.0.1
ping 30.0.0.1
ping 10.0.0.1
```


![student-ping-20](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/ACL/images/student%20ping%2020.png)
![studnet-ping-30](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/ACL/images/student%20ping%2030.png)
![student-ping-10](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/ACL/images/student%20ping%2010.png)
