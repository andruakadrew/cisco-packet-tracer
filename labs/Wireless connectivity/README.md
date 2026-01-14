# Wireless Connection Configuration
This lab demonstrates wireless configuration by connecting multiple end devices to a **Wireless Access Point**. The end devices are tested to confirm their connectivity through the PC Wireless tab and CLI.

## Network Topology
- 2 PC's
- 1 Smartphone
- 1 Laptop
- 1 Wireless Access Point


![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Wireless%20connectivity/images/topology.png)


## Configuring Devices
### End devices
All devices were given a static address within the _192.168.1_  network range. End devices were given the IP address _192.168.1.10 - 192.168.1.13_. All devices except for the Smartphone do not have a wireless
connection module installed. It's important to swap out the original module with a wireless module (e.g. WMP300N). 
### Steps to swap with wireless module
1. Click the device you want wireless connection on
2. Click the **Physical** tab to get a interactive image of the device
3. Power off the device
4. Drag and drop the original module under the list of available modules
5. Click the **WMP300N** module which provides a 2.4 GHz wireless interface that allows wireless connection to networks


![wireless-mopdule](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Wireless%20connectivity/images/wireless%20module.png)


### Wireless access point
To configure a wireless access point and have it accessable by devices within networks, the port which handles wireless connectivity must be configured.
- Turn Port Status **ON**
- Create a SSID for your wireless network
- Enable Authentication (e.g. WPA2-PSK)
- Create a PSK Pass Phrase


![port-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Wireless%20connectivity/images/wireless%20port%20config.png)


## Wireless Connections
To connect devices wirelessly to a network,
1. Choose the **PC Wireless** option under the **Desktop** tab
2. Click the **Connect** tab and Refresh
3. Choose the SSID of the network you wish to connect to and enter the PSK Pass Phrase


![connect](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Wireless%20connectivity/images/connect%20network.png)


4. Click the **Link Information** tab to ensure that the device has successfully connected to the network.


![confirm-link](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Wireless%20connectivity/images/confirm%20link.png)









