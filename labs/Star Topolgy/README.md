# Star Topolgy Lab
In this lab I configured a basic **star topology** network which relies on one central switch. All devices are able to communicate with each other through a central device, which is common in modern networks. Home Networks use a wireless router that serves as a
built-in switch for local devices. In small to medium sized offices, you will see similar setups to the lab that I have created today, where multiple end devices connect to a central switch. It's important to note, this type of topology is useful for small-medium
sized offices/workspaces. Larger workspaces require advanced topolgies such as the **three-tier hierarchical design**, due to the need of increased performance, security, scalability, redundancy, and easier management.

## Topology
- x1 Switch (2960-24TT)
- x6 PCs


![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Star%20Topolgy/images/topology.PNG)


## Deployment
In a **star topology**, the central device is commonly placed on a shelf or a wiring cabinet which is located in the middle of a room. This is done to minimize cable length/cost, simplify installation, and enhance the centralized management features that the
star topology benefits from. In my lab I deployed a switch and 6 pc's forming a circle around the switch. Each PC was connected to the switch's FastEthernet port using **straight-through copper**.


## Configuration
All PCs were assigned static IP addresses for simplicity, however in real life networks the IP addresses are usually assigned by a **DHCP** server. The range of IP addresses assigned was **192.168.1.10 - 192.168.1.15**.


![pc_ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Star%20Topolgy/images/pc-ipconfig.PNG)


## Verification
To ensure successful communication with other devices, I used the **ping** command to test connectivity. After pinging PC1 **192.168.1.10** from PC3 **192.168.1.11**, the results came back as 100% successful.


![ping_test2](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Star%20Topolgy/images/ping_test2.PNG)
