# Commands
The following is a list of commands used when configuring different nodes in the network

## Switches
* **enable**: previous to configurations
* **conf | configure ter | configure terminal**: either should work to enter 
configuration mode it depends on the version of paket tracer
* **hostname**: defines a name to the switch
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

* **configure a port(access ports/access interfaces) to allow specific vlan(s)**: This configurations are performed on switches cnnected to end user computers. un the following commands in config mode **interface fas | interface fastethernet** followed by the port #, configure mode using **switchport mode [type(in this case access)]** followed by the type, either trunk or access(remember trunk are switch to switch or switch to router or switch ot server and access is switch to computer), then specify vlans tha will be allowed **switchport access vlan | sw access vlan** followed by the vlan number, this last one means that the node/computer that connects to that port will be provided the IP address of the specified VLAN, you can either run **end** or **exit**, don't forget to save config with the command **copy run | do wri**
* **show vlan id #**: use it in config mode, shows vlan # info, shows the ports that are active in this VLAN
* **configure an existing interface VLAN to be an interface administration VLAN**: This procddure is done on all switches. The default interface for administration VLAN is VLAN 1, it is consider a security vulnerability to work with default values, so you should change this value. First enter config mode and then set a new interface administration VLAN with **interface vlan** followed by the number(any existing vlan), the configure an IP address with **ip add | ip address** followed by the IP address and a subnetmask, and then get the interface running with **no shutdown**. Using this VLAN you will be able to manage the switch using telnet protocol to connect remotely. All switches should have an IP address inside this VLAN so that an external computer can configure them. This will allow packages with same label to communicate with equipments that have same label by using telnet protocol.
* **configure trunk connections between switches**: This configuration has to be performed on all sides of a connection from switch to switch, if there is A connected to B, both A and B has to do this, also if there is A to C, A and C have to configure its trunk connection. Remember trunk are connections  switch to switch or switch to router or switch ot server and access is switch to computer, first run **interface fastethernet** followed by the port# that has a connection to a switch, then **switch mode trunk** and then **switchport trunk native vlan [vlan#]**
* **show interface trunk | sh interface trunk**: shows a list of trunk connections in this node

* **Select a range**: We configuring interfaces we can use the word range to configure a range of interfaces, for exampe **interface range fastethernet 0/x-y**

* **Configure stp mode**: Spannig tree protocol helps our network stay bucle free meaning it helps the network find the best route to a node of the network, cisco switches has STP(PVST) configured by default wich is slower than RSTP(RPVST). in configuration mode run **spannig-tree mode pvst|rapid-pvst**

* **configure security in ports**: Switches are consider a weak point in all networks since the ports can be reached physically, to mitigate this vulnerabillity you can run set restrictions to your ports using this commands: firsta access the port you want to secure, second configure the connection as an access connection **switchport mode access**, then **switchport port-security aging|mac-address|maximum|violation**

* **show spannig-tree**: you can identify the root switch in a spanning tree procotol network using this command, only the root switch will display something indicating it is the root, you can attach de word **summary** to this command to get another version of the info. Visually the root switch will have all its connections enabled

note that some of the configurations done by the previous commands like setting passwords at login and at access to config mode can be automated using a manager that will deoploy these configurations through an image 

* **More commands to configure a trunk connections in a port(s)**: **interface fastethernet [port#]**, **switchport mode trunk**, **switchport trunk allowed vlan [all/vlan#]**

* **More commands to configure and access connection in a port(s)**: **interface fastethernet [port#]**, **switchport mode access**, **switchport access vlan [vlan#]**

* **Configurar Protocolo VTP**: It is used to configure and manage VLANs, you can create delete, rename all VLANs from one place instead of doing it in each different node. There is three types, SERVER, CLIENT, TRANSPARENT. SERVER mode propagates VLAN info to other switches in the domain. CLIENT mode depens on a VTP SERVER, it cannot make any changes in VLAN info itself. **vtp mode [client|server|transparent]**, **vtp version [1|2]**, **vtp domain [domain(name)]**, **vtp password [password]**, when defining a switch as server and one or more switches as clients the domain(name) has to match on all nodes, optional: **show vtp status**

* **show running-config**: shows ports configurations, you can see if a port will feature a host or access connection