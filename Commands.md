# Commands
The following is a list of commands used when configuring different nodes in the network

## Switches
* **enable**: previous to configurations
* **conf | configure ter | configure terminal**: either should work to enter configuration mode it depends on the version of paket tracer
* **no ip domain lookup**: Saves time, if your switch is subscrided to a DNS service and you enter by error a command that doesn't exist it will waste time tyring to resolve it like if it was an IP addres
* **enable secret $password**: enables a password so that only authorized users can change configurations, it runs only in config mode, remember to save config by running "write" command after it. To remove password requirement just append a "no" keyword at the beggining. This password will be required after writing "enable" command
* **configure a password at starting CLI(login)**: write the following commands while in config mode, **"line console 0"**, then **"pass | password"** and append the password, in the next line execute command **"login"** followed by **"exit"**
* **do wri | wri | write**: saves configurations of the node to a persistent storage, it can be executed in user mode but to run it while in the config mode append "do" at the beggining
* **do sh mac-addres-table | sh mac-addres-table**: works in user mode but append "do" to run it in config mode to show a list of the mac addreses
* **reload**: restarts node, it runs in user mode
* **enable telnet(client/server application protocol to access terminal or remote systems on LANs or internet)**: Execute the following commands in config mode, **line vty 0 15**, **password** $append_passowrd, **login**, **exit**
* **create and configure a vlan**: enter the configuration mode and execute **vlan** followed by a name(usually a number), **name** followed by a name(letters, make sure to use a descriptive name), **exit**
* **show vlan**: shows a list of vlans configure in this node

note that some of the configurations done by the previous commands like setting passwords at login and at access to config mode can be automated using a manager that will deoploy these configurations through an image 