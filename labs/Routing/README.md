# Routing Lab
This lab focuses on the fundamentals of routing, which include configuring routers, assigning static routes, VLAN routing,
inter-VLAN, OSPF, EIGRP, NAT, etc). This project will expand upon my existing DHCP + DNS + LAN environment, by introducing a router into the environment.


## Router LAN Configuration
I added the 2911 router above the switch and conneced it to the switch using a _GigabitEthernet uplink._


![router-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router%20Topology.png)


![router-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router%20config.png)

- All configuration was done on the _GigabitEthernet0/0_ interface.
- IP Address: **192.168.1.1 255.255.255.0**


The router now acts as the boundary between my internal network and any future networks I add (e.g. simulated WAN's or additional VLANs).


## Updated Topology Connectivity
After connecting the router to the switch, all devices received the proper default gateway from the DHCP server:
- PCs received: **192.168.1.1 (via DHCP)**
- DNS continuted resolving names correctly
- Router responded to ICMP pings from the LAN

  
![router-ping-confirmation](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router%20connectivity.png)


Multiple client PC's were tested, and all replies came back successful.


# Connecting Two Networks
In this lab I connected two seperate networks together using static routing. 


![two-network-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Two%20network%20topology.png)


## Configuring Network A and Network B
For each network there is 
- 1 router
- 1 switch
- 2 Client PC's


The left side is **Network A**, and the right side is **Network B**. Both routers are connected together via the _Serial DTE_ cable.


---


### Router port configurations
The router is first connected to the switch port via FastEthernet. On the router config tab, I assign _FastEthernet0/0_ to the IPv4 address _192.168.1.1_, and subnet mask _255.255.255.0_. Do the same for Network B by assigning _FastEthernet0/0_ to the IPv4 address _192.168.2.1_.


**Router 0**
![router0-fa](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router0%20interface%20fa.png)



**Router 1**
![router1-fa](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router1-fa.png)


---


After connecting both routers, use the _Serial DTE_ cable to configure the port that was used. In this case the port is _Serial2/0_. For Network A, I assigned the port's IPv4 address to _10.0.0.1_, and subnet mask _255.0.0.0_. For Network's B router, I assigned the IPv4 address to _10.0.0.2_.


**Router 0**
![router0-serial](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router0%20interface%20sb.png)


**Router 1**
![router1-serial](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router1-serial.png)


---


### Static routing
An important step when ensuring proper communication between multiple networks is applying _static_ or _dynamic_ routing (In this lab I chose static routing). On each router, go to **Config > Static** and create a new static route. 


**Router 0**
![router0-static-routing](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router0%20static%20routing.png)
_Note:_ Router 0 points to the next networks address _192.168.2.0_, and the next hop is pointing to the next network router address.


**Router 1**
![router1-static-routing](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Routing/images/Router1-static-routing.png)
