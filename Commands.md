# Commands
The following is a list of commands used when configuring different nodes in the network

## Switches
* **enable**: previous to configurations

* **conf | configure ter | configure terminal**: either should work to enter 
configuration mode it depends on the version of paket tracer

* **hostname**: defines a name to the switch

* **no ip domain lookup**: Saves time, if your switch is subscrided to a DNS service and you enter by error a command that doesn't exist it will waste time tyring to resolve it like if it was an IP addres

* **enable secret $password**: enables a password so that only authorized users can change configurations, it runs only in config mode, remember to save config by running "write" command after it. To remove password requirement just append a "no" keyword at the beggining. This password will be required after writing "enable" command

* **configure a password at starting CLI(login), encrypt password and a message when password is incorrect**: write the following commands while in config mode, **"line console 0"** (this access the console port that is used to connect a computer to a device(switch), usually the port is a RJ-45 or in newer switches a USB port), then **"pass | password"** and append the password, in the next line execute command **"login"**, encrypt password using **service password-encryption**, display a message when password is incorrect with **banner motd "message"**

* **clock set [hh:mm:ss] [Day of Month] [Month] [Year]**: this sets a clock

* **do wri | wri | write**: equivalent to **copy running-config star** saves configurations of the node to a persistent storage, it can be executed in user mode but to run it while in the config mode append "do" at the beggining

* **do sh mac-addres-table | sh mac-addres-table**: works in user mode but append "do" to run it in config mode to show a list of the mac addreses

* **reload**: restarts node, it runs in user mode

* **enable telnet(client/server application protocol to access terminal or remote systems on LANs or internet)**: Execute the following commands in config mode, **line vty 0 15**, **password** $append_passowrd, **login**, **exit**. You would need to connect a console cable to the device that will connect to the switch

* **create and configure a vlan**: enter the configuration mode and execute **vlan** followed by a name(usually a number), **name** followed by a name(letters, make sure to use a descriptive name), **exit**, **end** to exit config mode. There is a default vlan(vlan 1), by default all FastEthernet ports associated to this vlan. When creating vlans you have to define what vlan is associated to an specific network interfaces/port

* **configure an existing interface VLAN to be an interface administration VLAN with an IP address**: This procddure is done on all switches. The default interface for administration VLAN is VLAN 1, it is consider a security vulnerability to work with default values, so you should change this value. First enter config mode and then set a new interface administration VLAN with **interface vlan** followed by the number(any existing vlan), the configure an IP address with **ip add | ip address [IpAddress] [subnetmask]**, and then get the interface running with **no shutdown**. Using this VLAN you will be able to manage the switch using telnet protocol to connect remotely. All switches should have an IP address inside this VLAN so that an external computer can configure them. This will allow packages with same label to communicate with equipments that have same label by using telnet protocol.

* **set up a default gateway in a switch**:  run `ip default-gateway [DefaultGatewayIpAddress]`

* **show vlan or show vlan brief**: shows a list of vlans configure in this node

* **show vlan id #**: use it in config mode, shows vlan # info, shows the ports that are active in this VLAN

* **copy run | copy running-config star | copy running-config startup-config**: it runs in user mode, either of this commands will make save vlan-s and interface configurations(ip address, gateway, subnetmask) in persistent storage so that a node saves all set up. This is the equivalent to **do wri** inside of config mode

* **configure a port(access ports/access interfaces) to allow specific vlan(s)**: This configurations are performed on switches cnnected to end user computers. un the following commands in config mode **interface fas | interface fastethernet** followed by the port #, configure mode using **switchport mode [type(in this case access)]** followed by the type, either trunk or access(remember trunk are switch to switch or switch to router or switch ot server and access is switch to computer), then specify vlans tha will be allowed **switchport access vlan | sw access vlan** followed by the vlan number, this last one means that the node/computer that connects to that port will be provided the IP address of the specified VLAN, you can either run **end** or **exit**, don't forget to save config with the command **copy run | do wri**

* **configure trunk connections between switches**: This configuration has to be performed on all sides of a connection from switch to switch, if there is A connected to B, both A and B has to do this, also if there is A to C, A and C have to configure its trunk connection. Remember trunk are connections  switch to switch or switch to router or switch ot server and access is switch to computer, first run **interface fastethernet** followed by the port# that has a connection to a switch, then **switch mode trunk** and then **switchport trunk native vlan [vlan#]**
* **show interface trunk | sh interface trunk**: shows a list of trunk connections in this node

* **Select a range**: We configuring interfaces we can use the word range to configure a range of interfaces, for exampe **interface range fastethernet 0/x-y**

* **Configure stp mode**: Spannig tree protocol helps our network stay bucle free meaning it helps the network find the best route to a node of the network, cisco switches has STP(PVST) configured by default wich is slower than RSTP(RPVST). in configuration mode run **spannig-tree mode pvst|rapid-pvst**

* **configure security in ports**: Switches are consider a weak point in all networks since the ports can be reached physically, to mitigate this vulnerabillity you can run set restrictions to your ports using this commands: firsta access the port you want to secure, second configure the connection as an access connection **switchport mode access**, then **switchport port-security aging|mac-address|maximum|violation**

* **show spannig-tree**: you can identify the root switch in a spanning tree procotol network using this command, only the root switch will display something indicating it is the root, you can attach de word **summary** to this command to get another version of the info. Visually the root switch will have all its connections enabled

* **configuring DTP**: Dynamic trunking protocol negotiates if the interface(port) should become an access port or trunk. Most switches have this protocol enabled by default. You can check if this protocol is enable or not with **show interfaces fastEthernet 0/x switchport | include Negotiation**, to turn it off there is two options firts set the interface as an access port **swichport mode access** if you need the port to be trunk mode and you want to turn this protocol off run this command when configuring the interface **switchport nonegotiate**, note that setting the port as trunk does not turn this protocol off

* **encapsulation**: When an ethernet frame travels through a vlan the switches(layer 2) don't know what vlan it belogns to so that is why have to configure one of two protocols in the switchport, either **802.1Q or ISL** beign the former the most common and is the default in some switches, in switches you can configure this using the command **switchport trunk encapsulation dot1q|isl** (dot1q for 802.1Q) and then **swictchport mode trunk**, note that if the encapsulation is set to auto or dynamic you can not configure the trunk mode. You can also configure encapsulation in subinterface of a **router(layer 3)** to be able to communicate between vlans(4 computers, 2 vlans,  connected to one swtich and the switch connected to a router) this protocol will help redirect a vlans trafic to another vlan if pinning a computer outside the origin vlan, run the commands in the router `interface fastEthernet 0/0.1`, `encapsulation dot1Q [vlanID] [navitve](this keyboard is optional as opposed to vlanId)`, **ip address [ipAddress] [subnetmask]**, the IP address doesn't have to match to an IP address of a copmuter connected to the VLAN but it could match the gateway of these of the client computers and the subnet mask doesnt have to match either

* **switchport trunk native vlan [vlanId]**: This command changes the native vlan which is usually set to vlan 1

note that some of the configurations done by the previous commands like setting passwords at login and at access to config mode can be automated using a manager that will deoploy these configurations through an image 

* **More commands to configure a trunk connections in a port(s)**: **interface fastethernet [port#]**, **switchport mode trunk**, **switchport trunk allowed vlan [all/vlan#]**

* **More commands to configure and access connection in a port(s)**: **interface fastethernet [port#]**, **switchport mode access**, **switchport access vlan [vlan#]**

* **Configurar Protocolo VTP**: It is used to configure and manage VLANs, you can create delete, rename all VLANs from one place instead of doing it in each different node. There is three types, SERVER, CLIENT, TRANSPARENT. SERVER mode propagates VLAN info to other switches in the domain. CLIENT mode depens on a VTP SERVER, it cannot make any changes in VLAN info itself. **vtp mode [client|server|transparent]**, **vtp version [1|2]**, **vtp domain [domain(name)]**, **vtp password [password]**, when defining a switch as server and one or more switches as clients the domain(name) has to match on all nodes, optional: **show vtp status**

* **show running-config**: shows ports configurations, you can see if a port will feature a host or access connection

## Static Routing
We can route VLANs or LANs, this means we can intercommunicate between networks either static or dinamically. Static routing means is all set up, only networks we set up are able to communicate. we can define different set ups, there are a lot of configurations we can have or ommit

### Statically route VLANs

#### A simple example
[One router, one swith and three networks/vlans with two computers each](./Clase/Practica3SencilloRouterRuteroVlans.pdf)

To sum up a "basic" setup would be just create vlans in all switches, note is possible to change/set up the native vlan in the switches. Lastly just follow the **IMPORTANT** step in the following section and it should work, note that default gateway address in PCs of a vlan has to match the ip address when declaring that Vlan in a sub-interface

#### Complex example
[two switches connected, one of them connected to a router and one computer connected to each switch](./Clase/practica3VlanRuteoComplejo.pdf)

This example is more complex because:
* we are assigning a vlan an ip with the following commands `interface vlan [vlanId]`, `ip address [ipAddress] [subnetmask]`

* In each switch we assign a different ip address for the same VLAN We also assign the same default gateway number to a each switch with command in config mode `ip default-gateway [DefaultGatewayIpAddress]`

* Remember the interface between router to switch and switch to swith has to be a **trunk** connection while switch to computer has to be an **access** connection, this is only configured in switches

* **(IMPORTANT)** Next we configured sub-interfaces in router, first access the sub-interface `interface fastEthernet 0/0.1`. Second, we define the vlan we allow`encapsulation dot1Q [vlanID] [navitve](this keyboard is optional as opposed to vlanId)`. Third, we assign an ip address `ip address [ipAddress] [subnetmask]` and here note that the ip address has to be the same as the default gateway used in the computers inside the vlan we are routing here.

### Statically route LANs
Read the [Instructions](./Clase/Practica4_B_LANRuteoEstatico.pdf)

The difference here is that we create dont create the networks virtually, they are created physically 

In order to do static routing we need **routers with serial ports** for example a router of model **2621XM** and add the ports **NM-4A/s** and interconnect to other routers using this serial ports only using serial **DCE** cables. Each router will have a connection to a different network(192.168.1.0/24, 192.168.2.0/24, 192.168.3.0/24. note the address with the 0 at the end means any ip adress that has that format and ends in any numer, 0 means any number we put /24 because the switches we have only have 24 ports) using fastethernet ports that will be connected to a switch and those switches will then be connected either to other switches or in the example presented in class [Practica4_B_LANRuteoEstatico](./Clase/Practica4_B_LANRuteoEstatico.pdf) they are connected to a computer using **Straight** cables between routers and switches and switches to computers.

Repeat these steps for all **routers**.

1. Configure all **FastEthernet** ports. Enable computers to send packets to other networks through the switch and the router by configuring the **fastethernet** port that connects the switch and the router. 
    * **int fa0/0**: access the interface
    * **ip address 192.168.1.1 255.255.255.0**: assign an ip address and the subnetmask, note this ip address will be the **default gateway** that will be **configured in the pc**, since there is three networks in this example the last two set of numbers vary per router like 1.X, 2.X and 3.X
    * **no shut**: this will turn the interface on

    *Imagine the case that there is 3 LANs an each LAN has ip of 200.10.10.0, 200.10.20.0 and 200.10.30.0, respectavaly. Bare in mind that *

2. Now we need to configure all **serial ports** that are connected to a router. Here we are just assigning an ip address to those ports so that we can direct packets from other networks to another network by linking the network ip address to serial port IP
    * **int s1/0**: acces the interface
    * **ip address 200.10.10.1 255.255.255.0**: If a router is using two serial ports you would configure a different IP address on each, so second port would have ip **200.10.10.2 255.255.255.0**, the ip addresses of these routers are also in a different network in each router for example the next router's last two set of numbers would be **20.X** and a third router **30.X**
    * **clock rate 64000**: this configures the frequency in the cable, since a cable as two sides we can only configure this command on one side, so only in one router the other router connected to this cable will only agree to this setting

3. Configure the static routing.
    * **ip route `networkAddress` `subnetMask` `serialPortIpAddress`**
Here we are telling the router how to find an external network, that means the `networkAddress` parameter points to an external network. `subnetMask` is belogns to that external network. `serialPortIoAddress` this address should be serial port Ip address from another router, this means we are Configuring Router A. Router B is directly connected to Router A, all that means is this `serialPortIpAddress` belongs to a port in router B. Example **ip route 192.168.2.0 255.255.255.0 200.10.20.0**

4.  At this moment we are able to send packets to another network, but not able to recieve a response from that network yet. Now we need to do the same in the opposite direction, meaning we need to point the network managed in Router A by doing this configurtations in Router B, example: **ip route 192.168.1.0 255.255.255.0 200.10.10.0**

5. If we need to connect from Router C to Router A but pass through Router B, you have to configure all ports you will say "all traffic sent to network A from network C first needs to be directed to port in Router B and all requests to network A received in router B needs to tavel to port in Router A"

To configure RIP protocol follow this steps: 

1. **router RIP**: this just enters RIP configurations context
2. **net [router or network addreesss]**: add ip adresses of networks and routers

## Dynamic Routing
It looks like it is pretty similar to static routing the only difference is in the routing command, you have to enter 0.0.0.0 for the network address and also 0.0.0.0 for the subnet mask, this zeros means enter address to any address, that means any address goes to this serail port address. Check the real short [example](./Clase//Practica4_C_DynamicRouting_defaultRoute_VLAN.pdf)

### Example Using RIP
Routing information protocol is based on distance costs, it determines the best route to take depending depending on the jumps it has to make to reach its destination, a jump happens when it reaches a router and goes to the next, that is one jump.

We use this protocol to announce all networks, that means inform all connected routers about the networks that are connected to this autonomus system(AS)

In this example we were given a [base topology](./Clase/Practica5_base_topology.pkt) 

Instructions

1. `show ip route`: We check what networks are directly connected to **R1**, the ones with the **C** label.

2. In this step we're going to inform other routers about the networks directly connected so they can be routed. Enter configuration mode in **R1** and enter command `router rip`, `version 2` version 2 supports networks without a class, that means the network doesn't necessarily define the default subnetmask. Now publish/announce the network(s) to the other routers(it will be done by RIP protocol automatically) `network 192.168.1.0`and any other network with the **C** label

3. Repeat step 1 and 2 in router **R2**

4. `show ip route`: Run it in **R1**, now you can se routes with an **R** label, in parenthesis you can se **(120/1)** this means it has less priority than static routes

## Show Comma nds

* `show ip route`: this command can run from privileged mode, it shows the current state of the routing table on a router. This is prefered when checking routes for example when working with RIP protocol

* `show ip interface`: it runs in privileged mode. It gets a detailed listing of all the IP-related characteristics of an interface, either a router or switch, etc.

