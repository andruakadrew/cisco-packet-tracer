# Basic Router Security Configuration
## Overview
When setting up a new router, security implementations should be one the first configurations made. The enable secret command is made for Cisco routers and sets a highly 
secure, non-reversible, hash password that is required for authentication into EXEC mode. The enable secret command replaces the legacy command enable password, which stores 
passwords in plain text or easily reversible type 7 encryption. It's also a good idea to modify the device's hostname to something relevant (e.g. Router10, R1, NYC-COR-R5),
enable security for the console port.


## Steps
- **Step 1:** Connect the PC to the routers console port using a console cable. This allows for direct physical management access to the devices _Command Line Interface (CLI)_.
- **Step 2:** Once the terminal has opened, enter _EXEC_ mode and _Configuration Terminal_. Create a relevant hostname for the router, in my lab I named it _R1_
```
hostname R1
```
- **Step 3:** Inside Configuration Terminal, create a secret passphrase. This protects the EXEC mode from unauthorized access. In my lab I used 'Cisco' for the secret passphrase
```
enable secret Cisco
```
- **Step 4:** The console port that allows direct management access should also have a password enabled.
```
line con 0
  password ccna
  login
```
- **Step 5:** The password for the console port is non-encrypted because we used the _password_ command. In order to encrypt the password for port login, use the command,
```
service password-encryption
```
- **Step 6:** To verify the adjustments, I used the command
```
show running-config
```
