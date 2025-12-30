# Home Network Simulation


## Overview
In this lab, I configured a network within a home environment using standard home networking equipment such as wireless routers and cable modems. In my lab I've implemented 1 wireless router, 1 cable modem, and
multiple end devices such as a TV, Laptop, and Office PC. In the home environment there is an office space, living room, and a bedroom. The cable modem serves as the translator between the home network and the
Internet.


![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/topology.png)


Components in this lab:
- Internet (ISP)
- Cable Splitter
- Cable Modem
- Wireless Router
- End Devices (PC, Laptop, TV)
Coaxial cable is used to connect the internet to the cable splitter. One line is used to connect to the Cable Modem while the other line is connected to the TV.


## Devices and Connections
1. **Internet (ISP)**
  - Provides access to the Internet
  - Connected via Coaxial cable


---
2. **Cable Splitter**
    - Spilts the incoming signal from the ISP cable into two paths
        - One path is connected to the Modem
        - One path is connected to the TV
     

    ---     
3. **Cable Modem**
- Receives the signal from the ISP and converts it into Ethernet signal
- Provides internet to the wireless router
- Connected to the WAN port


   ---
4. **Router**
  - Serves as the central networking device
  - Provides:
    - NAT
    - DHCP
    - Firewall
    - Wireless connectivity
  - Connects to:
    - Office PC (wired)
    - Bedroom PC (wired)
    - Laptop (wireless)
    - TV (wired)


## Wireless Router configuration
To configure this type of Wireless Router I used the web browser from the Office PC.


### Steps for configuration
1. **Assign an IP address to the Office PC via DHCP**
   -  DHCP is already a built in service that is automatically configured.
   -  The assigned IP address for the Office PC is _192.168.0.4_ and the Default Gateway _192.168.0.1_
![dhcp-assignment](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/officepc-dhcp.png)


---
2. **Open the web browser**
   - Input the Default Gateway into the URL to access the login prompt
   - Enter in the default credentials _admin_ for both the username and password
![setup-screen](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/setup-screen.png)


---
3. **Basic setting configuration**
There are a few settings I need to adjust in order to complete the router configuration.
  - Switch the maximum number of DHCP clients to 10
  - Change the default admin password
![dhcp-server-settings](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/dhcp-server-settings.png)
![router-password](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/router-password.png)


---
4. **Configure the wireless LAN**
For easy connectivity to the home network, I changed the Default SSID to something relevant (e.g. 'MyHomeNetwork'). In order to prevent unauthorized users from accessing the home network, I will enbale _WPA2
Persoanl_ under _2.4GHz_.
  - Switch the Default SSID to a Unique name
  - Enable WPA2 Personal under the 2.4GHz section
  - Create a password for the WPA2 policy
![ssid-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/2.4GHz.png)
![wpa2-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/wpa2.png)


---
### Laptop testing
To test connectivity to my network, I used the _PC Wireless_ feature under Desktop to test the wireless connectivity.
  - From the Laptop, I assigned an IP addresses via DHCP
  - I went to Desktop > PC Wireless
  - I clicked the _Connect_ tab to search for the Broadcasting network
  - Once found, I click connect and enter the Password for the Network.
  - The Laptop is now successfully connected to home network
![wireless-test1](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/wireless-connectivity1.png)
![wireless-test2](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/wireless-connectivity2.png)


To test the outside network server, I went onto the web browser and inputted _skillsforall.srv_ into the URL. A webpage responded back, confirming the server is accessibile from the Laptop.
![server-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/server-test-laptop.png)


---
### Office PC testing
To ensure the server is accessible to the Office PC, I requested a webpage via web browser
![officepc-server-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/server-test-officepc.png)


---
### Bedroom PC testing
To ensure proper connectivity to the home network:
- Assign an IP Address via DHCP
- Test the server _skillsforall.srv_ in the web browser
- In Command Prompt, ping the default gateway and neighboring devices
![bedroompc-testing](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/bedroompc-cli-testing.png)


---
### TV testing
To test connectivity to the TV, I simply turned the status ON
![tv-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Home%20Network/images/tv-test.png)
