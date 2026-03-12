# Fundamentals of Networking lab
## Overview
In this lab I configured a **star topology** and **extended star topology** that allows host to connect to a central device (e.g. switch, router). This topology is great for
small networks due to the ease of setup, easy to troubleshoot, and simple when adding devices. 


## Network Topology
Devices used in this lab:
- x1 2960 Switch
- x1 1941 Router
- x6 PCs


Below is a screenshot that shows a common Star Topology. All devices are connected to the switch via _Copper Straight-Through_. It's important to note for this topology, communication is limited to the local network.


![basic-network](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/basic-network.png)


An extended star topology is common in real world networks. When a router is implemented, all switches will connect to it and the router will then act as the central device.
A router allows devices within a local network to communicate with other networks.


![extended](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/basic-network-extenedstar.png)
