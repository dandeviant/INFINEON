何これ？

> p.s.
> 
> not including those network theoretical things like OSPF and shit. This is not university, alright? Just the important things you use in your daily operation.
> 
---

#### IOS management
> [! ]- Pro tip
> Managing Cisco IOS is like managing Linux, since Cisco IOS itself is built on Linux kernel, mainly Cisco IOS XE.\
> Learning Linux while you're at it will be a game changer in your IT life. Trust me...

| Commands | What for? |
| --- | --- |
| `dir flash:` | list `flash:` storage contents |
| `verify [/md5 OR /sha512] flash:[image file] [hash value]` | verify IOS image integrity before install |
| `request platform software package install switch all file flash:[image file]` | install image |
| `request platform software package clean switch all` | clean unused clutter files |
| `copy running-config startup-config` | Build config and save running config to flash |
| `erase startup-config` | Delete saved startup config in flash |

### Basic `show` commands, the important ones anyway...

| Commands | what for? | 
| --- | --- |
| `show running-config` | kinda explains itself, eh? |
| `show interface status` | show status of all physical ports and EtherChannel | 
| `show ip interface brief` | show IPv4 addressing, configurations and status on all ports and VLANs |
| `show vlan` | show all VLANs basic information |
| `show vlan group` | show all VLAN groups information |
| `show authentication sessions interface [int name] ` | show sessions by interface |
| `show authentication sessions mac [MAC address]` | show sessions by selected MAC address, same output as above |
| `show mac address-table address [MAC address]` | show connected devices by stated MAC address | 
| `show mac address-table interface [interface]` | show connected devices by stated interface |


### Basic configuration

> [! ]- Undoing configs
> - Reversing config commands is pretty simple. Just add `no` to the commands to remove it
> - For example:
> 	- `Switch(config-if)# no shutdown` - Start disabled ports
> 	- `Switch(config)# no vlan 99`  - Remove vlan 99, whatever the fuck that is
> 		- Small reminder; certain VLANs like VLAN 1, a default VLAN is unremovable.
> 		- Proof:
> 			```
> 			Switch(config)# no vlan 1
> 			Default VLAN 1 may not be deleted.
> 			```


| Commands | ##### |
| --- | --- |
| `conf t` | Move to configuration mode |
| **In Configuration mode** | **#####** |
| `interface [interface name]` | Move to stated interface for config |
| `vlan [vlan number]` | Create VLAN if not in list yet, and move to it for config |
| **Standard port config mode** | **#####** | 
| `ip address [IPv4] [mask value]` | Set IPv4 on physical ports. NOT APPLICABLE ON ACCESS LAYER SWITCH PORTS |
| `shutdown` | Shutdown and disable ports |
| `switchport access vlan [VLAN number]` | list the VLAN allowed to pass through the port. WILL REMOVE THE PREVIOUSLY ALLOWED LIST | 
| `switchport access vlan add [VLAN number]` | add VLANs to the allowed list

