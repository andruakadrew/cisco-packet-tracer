# SSH Configuration
## Overview
Secure Shell (SSH) is used for secure remote access to a host. Configuration on Cisco devices, such as a router or switch, can be performed using SSH. It works by connecting 
an end user to the command line of a device, where a username and password are required for authentication.

In this lab, I configured a router with a default gateway and an end PC to simulate the setup and verification of an SSH connection.

## Configuration
### Initial setup
Deploy a default gateway and a PC. Connect both of the devices using _Copper Straight-Through_. 

Assign static IP addresses to the Router interface and the PC.
- Router1 -> 192.168.10.1
- PC1 -> 192.168.10.10

---

### SSH
While in _Configuration mode_, create a domain name. This is required to generate crypto keys.
```
ip domain-name lab.local
```

Create a local user account
```
username admin privilege 15 secret cisco123
```

Generate RSA crypto keys
```
crypto key generate rsa
```

Force SSH version 2
```
ip ssh version 2
```

---

### Configure VTY lines to use SSH only
```
line vty 0 4
 transport input ssh
 login local
 exit
```

---

## Verification
Verify that SSH has been successfully configured on the router.
```
show ip ssh
```

Confirm the RSA key was generated successfully.
```
show crypto key mypubkey rsa
```

---

## Testing
From PC1, use the _ping_ command to test reachability to the routers IP address.
```
ping 192.168.10.1
```

SSH into the router with the authenticated credentials.
```
ssh -l admin 192.168.10.1
```
_Enter the password: cisco123_

The end users CLI should now land in the Routers privileged prompt (R1#)
