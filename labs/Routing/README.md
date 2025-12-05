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

