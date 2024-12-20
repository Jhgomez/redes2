# Commands
The following is a list of commands used when configuring different nodes in the network

## Switches
* **enable | en**: previous to configurations

* **conf t | configure ter | configure terminal**: either should work to enter 
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

* **configure security in ports**: Switches are consider a weak point in all networks since the ports can be reached physically, to mitigate this vulnerabillity you can run set restrictions to your ports using this commands: first access the port you want to secure, second configure the connection as an access connection **switchport mode access**, then **switchport port-security aging|mac-address|maximum|violation**. The ports can not be dynamic meaning the auto negotiation has to be turned off, in some switches just definining switchport mode to either trunk or access is enough but in others defining trunk mode is doesn't turn off auto negotiation so you have to also set **switchport nonegotate** to be able to run this configurations. **switchport port-security mac-address sticky**, **sw port-security maximum #quantity**

* **show spannig-tree**: you can identify the root switch in a spanning tree procotol network using this command, only the root switch will display something indicating it is the root, you can attach de word **summary** to this command to get another version of the info. Visually the root switch will have all its connections enabled

* **configuring DTP**: Dynamic trunking protocol negotiates if the interface(port) should become an access port or trunk. Most switches have this protocol enabled by default. You can check if this protocol is enable or not with **show interfaces fastEthernet 0/x switchport | include Negotiation**, to turn it off there is two options firts set the interface as an access port **swichport mode access** if you need the port to be trunk mode and you want to turn this protocol off run this command when configuring the interface **switchport nonegotiate**, note that setting the port as trunk does not turn this protocol off

* **encapsulation**: When an ethernet frame travels through a vlan the switches(layer 2) don't know what vlan it belogns to so that is why have to configure one of two protocols in the switchport, either **802.1Q or ISL** beign the former the most common and is the default in some switches, in switches you can configure this using the command **switchport trunk encapsulation dot1q|isl** (dot1q for 802.1Q) and then **swictchport mode trunk**, note that if the encapsulation is set to auto or dynamic you can not configure the trunk mode. You can also configure encapsulation in subinterface of a **router(layer 3)** to be able to communicate between vlans(4 computers, 2 vlans,  connected to one swtich and the switch connected to a router) this protocol will help redirect a vlans trafic to another vlan if pinning a computer outside the origin vlan, run the commands in the router `interface fastEthernet 0/0.1`, `encapsulation dot1Q [vlanID] [navitve](this keyboard is optional as opposed to vlanId)`, **ip address [ipAddress] [subnetmask]**, the IP address doesn't have to match to an IP address of a copmuter connected to the VLAN but it could match the gateway of these of the client computers and the subnet mask doesnt have to match either

* **switchport trunk native vlan [vlanId]**: This command changes the native vlan which is usually set to vlan 1

* **spannin-tree mode pvst|rapid-pvst**: Run it in conf mode. This lets you set the spanning-tree protocol(STP) to either normal(pvst) or rapid(rapid-pvst), convergence time is faster when reconfiguring the network using RSTP. You can check what mode of the protocol is configures either with **show running-config** or **show spainning-tree summary**, with the later option you can find out whether a switch is the root of the tree

note that some of the configurations done by the previous commands like setting passwords at login and at access to config mode can be automated using a manager that will deoploy these configurations through an image 

* **More commands to configure a trunk connections in a port(s)**: **interface fastethernet [port#]**, **switchport mode trunk**, **switchport trunk allowed vlan [all/vlan#]**

* **More commands to configure and access connection in a port(s)**: **interface fastethernet [port#]**, **switchport mode access**, **switchport access vlan [vlan#]**

* **Configurar Protocolo VTP**: It is used to configure and manage VLANs, you can create delete, rename all VLANs from one place instead of doing it in each different node. There is three types, SERVER, CLIENT, TRANSPARENT. SERVER mode propagates VLAN info to other switches in the domain. CLIENT mode depens on a VTP SERVER, it cannot make any changes in VLAN info itself. **vtp mode [client|server|transparent]**, **vtp version [1|2]**, **vtp domain [domain(name)]**, **vtp password [password]**, when defining a switch as server and one or more switches as clients the domain(name) has to match on all nodes, optional: **show vtp status**

* **show running-config**: shows ports configurations, you can see if a port will feature a host or access connection

* **Configure a loopback interface**: `interface Loopback0`, assign an ip address `ip address [ip address] [subnetmask]`

* **Add a description to an interface**: access the interface and do `description [some text]`

* **Enable asynchronous commands**: You will be able to execute or configure other commands while other configurations are being processed in the background. `line console 0`, `logging synchronous`, `exec-timeout 0 0`

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

* **(IMPORTANT)** Next we configured sub-interfaces in router, first access the sub-interface `interface fastEthernet 0/0.1`. Second, we define the vlan we allow`encapsulation dot1Q [vlanID] [navitve]("native" keyboard is optional as opposed to vlanId)`. Third, we assign an ip address `ip address [ipAddress] [subnetmask]` and here note that the ip address has to be the same as the default gateway used in the computers inside the vlan we are routing here.

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

To configure RIP protocol follow these steps: 

1. **router RIP**: this just enters RIP configurations context
2. **net [router or network addreesss]**: add ip adresses of networks and routers

## Dynamic Routing
It looks like it is pretty similar to static routing the only difference is in the routing command, you have to enter 0.0.0.0 for the network address and also 0.0.0.0 for the subnet mask, this zeros means enter address to any address, that means any address goes to this serail port address. Check the real short [example](./Clase//Practica4_C_DynamicRouting_defaultRoute_VLAN.pdf).

There is two types of protocols in dynamic routing
1. **IGP**: They are used when routing "local" networks. That means all networks in an autonomous systems belongs and are managed by one entity. The protocols **OSPF** **EIGRP** **RIP**

2. **EGP**: These protocols are used when routing to an "external" autonomous system, that means it helps an autonomous system administrated by an entity communicate with another AS that logically belongs to another entity. An example of this protcols is **BGP**

### Dinamically route LANs
When routing LANs we should use routers, switches doesnt require configurations in their terminal unless it is layer 3 switches. in this case they ports has to set to act as routers using command **no switchport** and then assigning an IP address to each port that is connected to a network and then configure either **RIP**, **OSPF** or **EIGRPl**

#### Example Using RIP
Routing information protocol is based on distance costs, it determines the best route to take depending depending on the jumps it has to make to reach its destination, a jump happens when it reaches a router and goes to the next, that is one jump.

We use this protocol to announce all networks, that means inform all connected routers about the networks that are connected to this autonomus system(AS)

In this example we were given a [base topology](./Clase/Practica5_no_guide_base_topology.pkt) 

Instructions

1. `show ip route`: We check what networks are directly connected to **R1**, the ones with the **C** label.

2. In this step we're going to inform other routers about the networks directly connected so they can be routed. Enter configuration mode in **R1** and enter command `router rip`, `version 2` version 2 supports networks without a class, that means the network doesn't necessarily define the default subnetmask. Now publish/announce the network(s) to the other routers(it will be done by RIP protocol automatically) `network 192.168.1.0`and any other network with the **C** label

3. Repeat step 1 and 2 in router **R2**

4. `show ip route`: Run it in **R1**, now you can se routes with an **R** label, in parenthesis you can se **(120/1)** this means it has less priority than static routes

#### Example Using OSPF
You can see the topology [here](./Lab/Clase6.pdf). Note we don't need to configure trunk or access interfaces in layer 2 switches at all.   Use following commands:

1. Configure PC IPs, the default gateway in each pc has to match the IP address we are going to assign to the port in the multi layer switch we have right on top of the PC

2. We need to assign the IP address in each multi layer switch, in order to do this we access the interface and run `no switchport`, this command puts the interface in "Layer 3" mode and makes it operate more like a router rather than a switch port, and then the regular `ip address` command, check the note above. In this example from left to right going one node by one node the the nodes will have the following IP addresses pc4 `192.168.10.2`(default gateway of `192.168.10.1`), fa0/2 `192.168.10.1`, fa0/1 `193.50.10.2`, fa0/1 `193.50.10.1`, fa0/2 `193.50.20.2`, fa0/1 `193.50.20.1`, fa0/2 `192.168.20.1`, pc5 `192.168.20.2`(default gateway of `192.168.20.1`)

3. First always activate intervlan `ip routing`

4. `router ospf [proccess id]`: proccess id can be any number

5. `network [network address] [wildcard] area [ospf area number]`: Repeat this step the same number as networks connected to this multilayer switch. Netork address is just the network address we are registering, wildcard is the subnetmask negated example: 255.255.255.0 negation is 0.0.0.255, and area parameter is a random number that **has to match** on all nodes. for example in multilayer switch 0 we would run `ip routing`, `router ospf 10`, `network 192.168.10.0 0.0.0.255 area 100`, next network `network 193.50.10.0 0.0.0.255 area 100`

6. Repeat steps 3-5 **on router 1**, `ip routing`, `router ospf 20`(different proccess number), `network 193.50.10.0 0.0.0.255 area 100`(same are), next network `network 193.50.20.0 0.0.0.255 area 100`(same area)

7. Repeat step 6 **on router 3**

### Dinamically Route VLANs
We have examples using EIGRP and OSPF [here](./Lab/Clase6.pdf). Note here we use multilayer switches which are layer 3 devices just like routers but routes are used when routing physical networks.

It is important to activate intervlan communication, if using dynamic routing, before configuring any **IGP** protocol by using the following command if using layer 3 swqitches
* **`ip routing`**: run it in config mode

#### Example using OSPF
As you can tell it is possible to use any routing protocol in any context, meaning they can be used either in LANs or VLANs, an example in LANs context was already documented, but now we are implementing the example [here](./Lab/Clase6.pdf) that uses OSPF with VLANS. Follow these steps

1. Create vlans on both switches, VLAN10 and VLAN20

2. Assign ip addresses on both computers. Pay attention to the default gateway, the following is an example, the switch on top of computer of VLAN10 will assign an ip address on the following step, the **VLAN10 in that switch** will act as the default gateway so this is the only IP address we need to make sure it matches, **it has to match the default gateway of the computer below it**, the other VLAN20 can have any ip address, this will work the same way on the other switch but there VLAN20 has to match with defaul gateway of computer below it

3. Access the VLAN interface and assign an ip address and subnet mask

4. configure access mode in the switch in connections between switch and PCs, only allow VLAN 10 in one side and only VLAN20 on the other switch access port

5. configure trunk mode between switches, the only difference with switches of layer 2 is that switches of layer 2 has encapsulation mode dot1q already set up, but in layer 3 switches we have to start with setting that up with this command `switchport trunk encapsulation dot1q` and then just the regular commands

6. run `ip routing` to enable routing, configure ospf the routing protocol run `route ospf [proccessId]` and now register both all vlans in this network, remember this protocol stores all the map of all networks, run `network [networkAddress] [wildcard(negated subnetmask)] area [areaNumber]` the area number has to  be the same on any network registered inside the AS. Repeat on both switches

#### Example using EIGRP
This setup is pretty much the same as the previous example right before this one, meaning we need to create vlans and assign ips to vlans and pcs as describred in the example right above this one, meaning follow the steps from 1-5. The example was taken from [here](./Lab/Clase6.pdf). and then replace step 6 with these instructions

6.  run `ip routing` to enable routing, configure eigrp the routing protocol run `route eigrp [AS_Id]`, the AS id has to  be the same on any network registered inside AS,  and now register both all vlans in this network, remember this protocol stores all the map of all networks, run `network [networkAddress]`, note this protocol doesn't need a negated wildcard and lastly you can optionally run `no auto-autosummary`, without this command the networks from the switch interfaces would clasify its networks either as A, B or C to its neighboors. Repeat these on both switches

1. First always activate intervlan `ip routing`

2. `router eigrp [as number]`

3. `network [network address]`

4. no auto-summary

## Show Commands

* `show ip route`: shows RIP, EIGRP, OSPF networks. This command can run from privileged mode, it shows the current state of the routing table on a router. This is prefered when checking routes for example when working with RIP protocol

* `show ip interface`: it runs in privileged mode. It gets a detailed listing of all the IP-related characteristics of an interface, either a router or switch, etc.

* `sh vtp status`

* `sh vlan brief`

* `sh interfaces trunk`

* `sh ethernetchannel summary`: shows channels configured with LACP or PAGP

* `sh ip eigrp/ospf/rip neighbors`

* `sh ip interface brief`: shows vlans, fa, gi, and other ports or interface that has been assigned an IP address

* `show port-security interface interface#`: shows security config of the interface selected

* `ip config-all`: shows info like mac address of the NIC

## Ethernet Channel - PAGP/LACP
This two protocols allows us to create ethernet channels easily. Ethernet channels are a logical group of physical connections that will be treated as a single logical connection. These two protocols are used between switch to switch to group two or more ethernet connections to be treated as a single connection

### LACP
Has two modes `active` or `passive`, passive to passive connections don't work, all other combinations do. One switch has one mode and the other has the other mode. Usefull when working with different brands switches as is a standar protocol

### PAGP
Does exactly the same as LACP but is pattented by Cisco. Has two modes `desirable` and `auto`

### Commands
Configure them using following commands:

1. Select range `interface range [type(fa/gb)] [range(example: 0/1-4)]`
2. Select protocol `channel-protocol pagp/lacp`
3. Select group and mode, you can create groups only from 1-6.  For LACP `channel-group [1-6] mode [active/passive]`, for PAGP `channel-group [1-6] mode [dessirable/auto]`

you can then display the setup with: `show etherchannel summary` 

## Configure DHCP
DHCP protocol helps us simplify the proccess of connecting devices and managing network resources by providing IP addresses and other configuration parameters automatically. We are using the topology created [here](./Clase/practica6_no_tiene_instrucciones_ver_commands.pkt), in this exercise we are configuring the DHCP service in router 

0. Be aware that switch to switch connections has to be set to trunk, switch to router has to be trunk, router to switch just make sure is up, router to router just make sure is up, and switch to computer has to be access mode

1. Since we are configuring VLANs in this example we have to configure the the subinterfaces just like in the router-on-a-stick example in the static routing examples(complex example), this is done to route vlans. So enter SW1 and access gigabitEthernet 0/1 and access sub interface .10 `int gi 0/1.10`, `encapsulation dot1q 10`, set the vlan10 gateway in this interface `ip address 192.168.10.1 255.255.255.0`. do the same with vlan 20 `int gi 0/1.20`, `encapsulation dot1q 20`, set the vlan10 gateway in this interface `ip address 192.168.20.1 255.255.255.0`. turn them up `no shutdown`, check configuration with `sh ip interfaces brief`

2. configure DHCP, in configuration mode run `ip dhcp pool [vlanId]`(vlanId = vlan10/vlan20) set the defaul router(it has to be the same as the default gateway for the vlan) `default-router 192.168.10.1`, set the network we are serving with this pool `network 192.168.10.0 255.255.255.0` and in case we want to set up a dns server do `dns-sercver 192.168.10.5`. Do the same for vlan20 `ip dhcp pool [vlanId]`, `default-router 192.168.20.1`, `network 192.168.20.0 255.255.255.0`, `dns-sercver 192.168.20.5`. After setting them up exclude some ip addresses in configuration mode with `ip dhcp excluded-address 192.168.10.1 192.168.10.2 192.168.10.5 192.168.20.1 192.168.20.2 192.168.20.5`, note we are exlcuding the defaul gateway and the dns server ip address. Note that is not necessary to create a dns server for each vlan, we can acuatlly use the same for all, which is the most common thing to do. We could have a server vlan and using rip protocol we could allow all vlans to communicate with a network that is network only for servers and DNS would be added to that newtwork for example a dns with ip 192.168.100.5

3. configure seral port in R1 `int serial 0/1/0`, `ip address 192.168.1.225 255.255.255.252`, `no shutdown`

4. we will use RIP to share other networks so in R1 do `router rip`, `version 2`, `network 192.168.1.224`,`network 192.168.10.0`, `network 192.168.20.0`

5. now configure access connections in sw1, `int fa 0/1`, `sw access vlan 10`, `int fa 0/11`, `sw access vlan 20`

6. configure trunk connction in sw3, `int g 0/1`, `sw mode trunk`, check it with `sh interfaces trunk`

7. check the PC0 and PC4 are working, go to 'IP configuration' section and select 'DHCP' on both computers and you will see an ip corresponding to the right vlan assigned to the computer

8. Here we are configuring vlans statically. Configure R2 basically same proccess as R1(step 1,2, 3 and 4). `int gi 0/1.30`, `encapsulation dot1q 30`, set the vlan30 gateway in this interface `ip address 192.168.30.1 255.255.255.0`. `no shutdown`. `int gi 0/1.40`, `encapsulation dot1q 40`, set the vlan30 gateway in this interface `ip address 192.168.40.1 255.255.255.0`. `no shutdown`, check configuration with `sh ip interfaces brief`

9. configure DHCP.  Vlan30 `ip dhcp pool vlan30(the name of the pool)`, `default-router 192.168.30.1`(same as vlan default gateway), `network 192.168.30.0 255.255.255.0`, `dns-sercver 192.168.30.5`. Vlan40 `ip dhcp pool vlan40`, `default-router 192.168.40.1`(same as vlan default gateway), `network 192.168.40.0 255.255.255.0`, `dns-sercver 192.168.40.5`. After setting them up exclude some ip addresses in configuration mode with `ip dhcp excluded-address 192.168.30.1 192.168.30.2 192.168.30.5` `ip dhcp excluded-address 192.168.40.1 192.168.40.2 192.168.40.5`, note we are exlcuding the defaul gateway and the dns server ip address

3. configure seral port in R2 `int serial 0/1/0`, `ip address 192.168.1.226 255.255.255.252`, `no shutdown`

10.  RIP to share other networks so in R2 do `router rip`, `version 2`, `network 192.168.1.224`,`network 192.168.30.0`, `network 192.168.40.0`

11. make sure router to router and router to switch connections are up, no need configure any protoocol in this type of connections

12. be aware vtp is layer 2 and it doesn't work through layer 3 devices, this means we need to create another vtp server in sw4, vtp domain and password is same, usac

14. Go to 'IP configuration' section and select 'DHCP' on all right side computers and you will see an ip corresponding to the right vlan assigned to the computer

## Configure Proyecto 1

1. configure LACP in MS 1, 2, 3 and 7, add a power supply to 3650 switches. `interface range [type(f/g)] [range(example: 0/1-4)]`, select protocol `channel-protocol pagp/lacp`, select group and mode, you can create groups only 
from 1-6. For LACP `channel-group 1 mode active`

3. configure vtp. We will have two servers on each side, only access and routing layer cares about VLANs, the core layer doesn't configure any VLAN set up, but also be aware "vtp" protocol can not travel throuhg layer 3 devices, such as swichports acting as routers or a router itself, that is why we will have two servers, one on each side. MS6 on the left side and MS10 on the right side. `vtp mode server`, `vtp version 2`, `vtp domain juan`, `vtp password juan`. Now we will configure the clients on each side. Left side clients are MS4, MS5, SW0 and S1. Right side clients are MS8, MS9, SW2 and SW3. Do `vtp mode client`, `vtp domain juan`, `vtp password juan`

4. Create vlans on server switches MS6 and MS10. `vlan 10`, `name orange`, `vlan 20`, `name green`

5. Configure Access connections. This is only done in layer 2 switches interfaces connected to end devices and they are considered the Access layer. On the left side access layer is SW0 and SW1, on the right side is SW2 and SW3. On ports connected to end devices run `int fa0/#`, `sw mode access`, `sw access vlan [vlan#]`

6. Configure trunk connections. This is done in connections between access and routing layer as well as within the routing layer itself. Routing layer on the left side is the ports in MS4, MS5 and MS6 connected between each other and to access layer switches as well as the ports in switches of access layer(SW0 and SW1) that are connected to routing layer switches, we find the same "pattern" on the right side with access layer swit, ches(SW2 and SW3) ports connected to routing layer switches(MS8, MS9, MS10) as well as connections withing them. Run `int fa0/#`(you can select a range instead of an interface at a time), `sw trunk encapsulation dot1q`, `sw mode truk`, `sw trunk allowed vlan 10,20`.

7. Configure webssite on SERVER WEB, basically server web will configure a DNS service so we can host a website in this server

    * First go the "Desktop" tab and go to "IP Configuration", IP address will be "150.0.3.254", subnet mask "255.255.255.0", default gateway "150.0.3.1".

    * On the "Services" tab check what service we don't need that can be turned off like the email service then turn up "DNS" service. enter the name of how you want to call your website, I used "pro.com", the "address" has to be the same address as the ip address in prev step "150.0.3.254", select "a record". Save it

### HSRP(Just a quick note)
This is a "redundancy" protocol for stablishing a fault-tolerant default gateway. If configuring LANs just take the pair of routers/switches that will be used to simulate a single virtual router

8. Configure DHCP. It was required that for DHCP there is two servers at the top, the left side server provides ip addresses for left side of topology and right side to right side. Just as a note DHCP works by sending a broadcast at the beginning, this is what is known as "DHCP discover", again it is just a broadcast

    * assign IP address to the server in the "desktop" tab go to "IP Configuration", DHCP1 ip address will be "170.0.1.254", subnet mask "255.255.255.0", default gateway "170.0.1.1". DHCP2 "180.0.2.254", subnet mask "255.255.255.0", default gateway "180.0.2.1"

    * Enter the server and activate DHCP in the "services" tab

    * Add a pool for each vlan. On DHCP1 
    
    "pool Name" = VLAN10. 
    "DNS server" = 150.0.3.254
    "dafault gateway" = 192.168.5.1(in this case it is always the first ip address in the network)
    "subnet" = 255.255.255.192
    "start ip address" = is 192.168.5.4, 
    
    next pool:
    VLAN20
    150.0.3.254
    192.168.5.65
    255.255.255.192
    192.168.5.68

    On DHCP2

    VLAN20
    150.0.3.254
    192.168.5.129
    255.255.255.192
    192.168.5.132

    VLAN10
    150.0.3.254
    192.168.5.193
    255.255.255.192
    192.168.5.196

    * If VLAN node that is being provided a dynamic IP is connected directly through a router or layer 3 switch toj a DHCP server we would just have to access the interface that is connected to the DHCP server and run command `ip helper-address [ipOfDHCPServer]` but in this example since we are using vlans in different buildings and that means they are not connected directly first we need configure helper in the vlans in the frontier of routing layer, we will do it on step 8

9. Configure VLANS, at the same time we will configure HSRP, and set IP helper which is part of DHCP configurations. First we will access the vlan interface then assign an ip address to the vlans and finally configure HSRP. To configure HSRP we pair two routing layer switches, so we could say this configuration lives in the routing layer. HSRP virtual router IP address will be the gateway of our VLANS. On the left side we will configure MS4, MS5. On the right side MS8, MS9. On each left router will be active and right will be passive. On MS4 run `int vlan 10`, `ip address 192.168.5.2 255.255.255.192`, `standby [anId(a random ID, must commonly vlan# is used, use 10)] ip [vlanGatewayIpAddress use 192.168.5.1]`, `standby [the id we configured in prev step, 10] priority [a number, default priority is 100, use 150]`, `standby 10 preempt` `ip helper-address 170.0.1.254`, `int vlan 20`, `ip address 192.168.5.66 255.225.255.192`. `standby 20 ip 192.168.5.65`, `standby 20 priority 150`, `standby 20 preempt` `ip helper-address 170.0.1.254`. On MS5 `int vlan 10`, `ip address 192.168.5.3 255.255.255.192`, `standby 10 ip 192.168.5.1` `ip helper-address 170.0.1.254`, `int vlan 20`, `ip address 192.168.5.67 255.225.255.192`, `standby 20 ip 192.168.5.65`, `ip helper-address 170.0.1.254`. On MS8 `int vlan 20`, `ip address 192.168.5.130 255.255.255.192`, `standby 20 ip 192.168.5.129`, `standby 20 priority 150`, `standby 20 preempt`, `ip helper-address 180.0.2.254`, `int vlan 10`, `ip address 192.168.5.194 255.225.255.192`, `standby 10 ip 192.168.5.193`, `standby 10 priority 150`, `standby 10 preempt`, `ip helper-address 180.0.2.254`. On MS9 `int vlan 20`, `ip address 192.168.5.131 255.255.255.192`, `standby 20 ip 192.168.5.129`, `ip helper-address 180.0.2.254`, `int vlan 10`, `ip address 192.168.5.195 255.225.255.192`, `standby 20 ip 192.168.5.193`, `ip helper-address 180.0.2.254`.

10. Assign ip addresses to all ports on switches MS0, MS1, MS2, MS3, MS4, MS5, MS7, MS8, MS9, MS11 that are connected to other switches doing routing. Access the right interface for example for ethernet channel `int port-channel #channel`, `no sw` so we can get a router's functionallity, `ip addreas [ipaddress] [subnetMask]` assign the ip address as indicated in the topology

11. Configure EIGRP on switches MS0, MS1, MS2, MS3, MS4, MS5, MS7, MS8, MS9, MS11. firts enter configuration mode and activate dynamic routing functionalllity with `ip routing`, now get the networks the swicht is connected to with `do sh ip route`, take note of all network labeled with a letter "c", start eigrp `router eigrp [autonomous system number, we will use 100]` and for each of those networks do `network [ip address] [negated subnetMask]`

12. This step may vary depending on the networks and subnets used to connect the switches acting as routers, basically all those switches would be the core layer but at least in this specific case we just need to turn of "auto-summary" in two switches, the switches that hold the vlan with the default gate way ip address set to it, the two virtual switches we created but that means we actually have to configure 4 switches, **MS4, MS5, MS8 and MS9**. Enter these switches and do `router eigrp 100`, `no auto-summary`. If no auto summary is set then if there is a subnet connected indirectly, meaning is recongnized by eigrp through other networks inside the autonomous system(AS) like the case of the network `192.168.5.0` being divided into 4 subnets then the routing table  will not explicitly add them to the table but they will be added implicitly like either `192.168.0.0` or `192.168.0.0 null`, in our case it was like the later network with a "null" tag, and this will cause any request made to networks with a similar ip address that are not specifically added to the table to fail automatically, they will not travel further than the switch that has this in its table so waht we need to do is tell the switch to don't that automatically with the command we mentioned, just note that with the ip address without the null communication could have been stablished between left and right side. For example on the left side without `no auto-summary` the table would have included `192.168.5.0/26` and `192.168.5.64/26` with a "C" label under `192.168.5.0/24` but we would have also found a `192.168.5.0/24 Null0` in the same section which as mentioned, causes the requests to fail because it doesn't explicitly recongnizes the other subnets

12. Turn DCHP on end devices(computers) in the 

13. // TODO, configure ACLs on routing layer switches

## Remember
Routers and Layer three switches can route VLANs and LANs, in this document you will find examples with all of these scenarios but we are putting them together in this section

1. Routing vlans with routers, here you have to do subinterface routing access the so for example access a gigabitethernet port in a router which should be connected to a switch and do something similar to this `int g0/1.10`(the subinterface can .20, .30, etc), `encapsulation dot1q [vlan#]`, set the vlan gateway in this interface `ip address [gatewayIp] [subnetmask]`, `no shutdown`. This process should be done with all routers so they can route any network that is directly connected to it, usually is connected through a layer 2 switch, this should be enough after this you just have to configure static or dynamic routing and trunk and access connections as needed in switches, a good example is "Configure DHCP" example

2. Routing valns with layer 3 switches, a good example is the [this](./Lab/practica2EIGRP.pkt) the instructions are [here](./Lab/Practica2_README.md), basically you have to, again, configure access and trunk connections but this time since this is switches connecting different buildings we have to do a trunk connection between different buildings multilayer switches which are doing the routing using an ethernet channel, the vlans in the routers that are doing the routing in each building has to assing an ip address to the vlan just like in "1." instructions, do `int vlan [vlan#]`, `ip address [vlanDefaultGateway] [subnetMask]`, default gateway should be the same default gateway in computers inside the vlan we want to route, this switch could have a lot of configurations below but the would only configure trunk or access mode as needed. sw to sw is trunk, sw to computer is access. The different buildings are connected with an ethernetchannel, in here we will do something that we do when configuring LANS which is assignig an ip address to the channel that connects to the other building multilayer switches basically configure LACP, do a `no switchport` so it acts as a routing port and we can assign an IP address which is what is similar to what we do when routing LANs assign an IP address to this channel. Then with eigrp you have to announce all the networks it is connected to, the networks below, above, right and left

3. Routing LANs with routers, router interfaces can assign different ip for different subinterfaces only with vlans but with physical networks a single port can only be one gateway as opossed with vlans, so this is all you need to be aware

4. Routing LANS with switches, is the same story(only one LAN per switchport) as the prev number("3.") with the only difference that we need to enable routing `no switchport` to the interface, in this insturctions and the previouse("3.") we would assign an ip address to the interface that connects with the network, then we can connect several routers using static or dynamic routing by adding an ip address(same network) to each side of a router meaning we would use different ip addresses on the same network and then configure the routing protocol

5. One thing that was hard to understand is that the routing node, either a router or a layer 3 switch, can have connections to other layer3 switches which at the same time has connection to other layer 2 switches and those other switches besides the routing node only requires trunk or access connection configured(if routing vlans) or if routing LANs they don't need any configuration at all, the routing node could be actually a pair of layer 3 switches this would be the case if we are using a HSRP, this protocol creates a single router, ofter called a virtual router which serves as a default gateway