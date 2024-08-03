# Commands
The following is a list of commands used when configuring different nodes in the network

## Switches
* **enable**: previous to configurations
* **conf | configure ter | configure terminal**: either should work to enter configuration mode it depends on the version of paket tracer
* **no ip domain lookup**: Saves time, if your switch is subscrided to a DNS service and you enter by error a command that doesn't exist it will waste time tyring to resolve it like if it was an IP addres
* **enable secret clas**: (pending investigation)
* **do wri | wri | write**: saves configurations of the node to a persistent storage, it can be executed in user mode but to run it while in the config mode append "do" at the beggining
