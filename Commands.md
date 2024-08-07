# Commands
The following is a list of commands used when configuring different nodes in the network

## Switches
* **enable**: previous to configurations
* **conf | configure ter | configure terminal**: either should work to enter configuration mode it depends on the version of paket tracer
* **no ip domain lookup**: Saves time, if your switch is subscrided to a DNS service and you enter by error a command that doesn't exist it will waste time tyring to resolve it like if it was an IP addres
* **enable secret $password**: enables a password so that only authorized users can change configurations, it runs only in config mode, remember to save config by running "write" command after it. To remove password requirement just append a "no" keyword at the beggining. This password will be required after writing "enable" command
* **configure a password at starting CLI(login)**: write the following commands while in config mode, **"line console 0"** (this access the console port that is used to connect a computer to a device(switch), usually the port is a RJ-45 or in newer switches a USB port), then **"pass | password"** and append the password, in the next line execute command **"login"** followed by **"exit"**
* **do wri | wri | write**: equivalent to **copy running-config star** saves configurations of the node to a persistent storage, it can be executed in user mode but to run it while in the config mode append "do" at the beggining
* **do sh mac-addres-table | sh mac-addres-table**: works in user mode but append "do" to run it in config mode to show a list of the mac addreses
* **reload**: restarts node, it runs in user mode
* **enable telnet(client/server application protocol to access terminal or remote systems on LANs or internet)**: Execute the following commands in config mode, **line vty 0 15**, **password** $append_passowrd, **login**, **exit**. You would need to connect a console cable to the device that will connect to the switch
* **create and configure a vlan**: enter the configuration mode and execute **vlan** followed by a name(usually a number), **name** followed by a name(letters, make sure to use a descriptive name), **exit**, **end** to exit config mode. There is a default vlan(vlan 1), by default all FastEthernet ports associated to this vlan. When creating vlans you have to define what vlan is associated to an specific network interfaces/port
* **show vlan or show vlan brief**: shows a list of vlans configure in this node
* **copy run | copy running-config star | copy running-config startup-config**: it runs in user mode, either of this commands will make save vlan-s and interface configurations(ip address, gateway, subnetmask) in persistent storage so that a node saves all set up. This is the equivalent to **do wri** inside of config mode
* **configure a port(access ports/access interfaces) to allow specific vlan(s)**: run the following commands in config mode **interface fas | interface fastethernet** followed by the port #, configure mode using **switchport mode** followed by the type, either trunk or access, then specify vlans tha will be allowed **switchport access vlan | sw access vlan** followed by the vlan number, this last one means that the node/computer that connects to that port will be provided the IP address of the specified VLAN, you can either run **end** or **exit**, don't forget to save config with the command **copy run | do wri**
* **show vlan id #**: use it in config mode, shows vlan # info, shows the ports that are active in this VLAN


note that some of the configurations done by the previous commands like setting passwords at login and at access to config mode can be automated using a manager that will deoploy these configurations through an image 