pasos para configurar

### Crear VLANs en Switches

#### M2, T3, T9 y Biblioteca Central
```bash
Switch>en
Switch#conf t
Switch(config)#hostname <M2, T3, T9, BCENTRAL> 
M2(config)#vlan 15
M2(config-vlan)#name SOPORTE
M2(config-vlan)#exit
M2(config)#vlan 25
M2(config-vlan)#name VISITANTES
M2(config-vlan)#exit
M2(config)#vlan 35
M2(config-vlan)#name RECURSOS
M2(config-vlan)#exit
M2(config)#do write
```

### Asignar VLAN a interfaces

Asignar los puertos donde están conectados los dispositivos finales a las VLANs correspondientes

#### M2
```bash
M2(config)#int f0/10
M2(config-if)#switchport mode access
M2(config-if)#switchport access vlan 35
M2(config-if)#exit
M2(config)#int f0/11
M2(config-if)#switchport mode access
M2(config-if)#switchport access vlan 25
M2(config-if)#exit
M2(config)#do write
```

#### T3
```bash
T3(config)#int f0/10
T3(config-if)#switchport mode access
T3(config-if)#switchport access vlan 35
T3(config-if)#exit
T3(config)#int f0/11
T3(config-if)#switchport mode access
T3(config-if)#switchport access vlan 15
T3(config-if)#exit
T3(config)#do write
```

#### T9
```bash
T9(config)#int f0/10
T9(config-if)#switchport mode access
T9(config-if)#switchport access vlan 25
T9(config-if)#exit
T9(config)#int f0/11
T9(config-if)#switchport mode access
T9(config-if)#switchport access vlan 35
T9(config-if)#exit
T9(config)#do write
```

#### Biblioteca central
```bash
BCENTRAL(config)#int f0/10
BCENTRAL(config-if)#switchport mode access
BCENTRAL(config-if)#switchport access vlan 35
BCENTRAL(config-if)#exit
BCENTRAL(config)#int f0/11
BCENTRAL(config-if)#switchport mode access
BCENTRAL(config-if)#switchport access vlan 25
BCENTRAL(config-if)#exit
BCENTRAL(config)#do write
```

### Configurar Enlaces Troncales

Configurar los puertos de enlace que conectan este Switch normal a un Switch MultiLayer como troncales, para que permitan pasar el tráfico de todas las VLANs.

#### M2, T3, T9 y Biblioteca Central
```bash
M2(config)#int f0/1
M2(config-if)#switchport mode trunk
M2(config-if)#switchport trunk allowed vlan 15,25,35
M2(config-if)#exit
M2(config)#do write
```

### Crear VLANs en Switches MultiLayer

#### M2, T3, T9 y Biblioteca Central
```bash
Switch>en
Switch#conf t
Switch(config)#hostname <MLSM2, MLST3, MLST9, MLSBCENTRAL> 
MLSM2(config)#vlan 15
MLSM2(config-vlan)#name SOPORTE
MLSM2(config-vlan)#exit
MLSM2(config)#vlan 25
MLSM2(config-vlan)#name VISITANTES
MLSM2(config-vlan)#exit
MLSM2(config)#vlan 35
MLSM2(config-vlan)#name RECURSOS
MLSM2(config-vlan)#exit
MLSM2(config)#do write
```

### Configurar Enlaces Troncales MultiLayer

Configurar los puertos de enlace que conectan este Switch normal a un Switch MultiLayer como troncales, para que permitan pasar el tráfico de todas las VLANs.

#### M2, T9 y Biblioteca Central
```bash
MLSM2(config)#int range f0/1-5 // Este rango nos servira luego para configurar LACP
MLSM2(config-if-range)#switchport trunk encapsulation dot1q
MLSM2(config-if-range)#switchport mode trunk
MLSM2(config-if-range)#switchport trunk allowed vlan 15,25,35
MLSM2(config-if-range)#exit
MLSM2(config)#do write
```

#### T3
```bash
MLST3(config)#int range f0/1-13 // Este rango nos servira luego para configurar LACP
MLST3(config-if-range)#switchport trunk encapsulation dot1q
MLST3(config-if-range)#switchport mode trunk
MLST3(config-if-range)#switchport trunk allowed vlan 15,25,35
MLST3(config-if-range)#exit
MLST3(config)#do write
```

### Configurar Interfaces VLAN en Switch MultiLayer

#### M2
```bash
MLSM2(config)#int vlan 35
MLSM2(config-if)#ip address 192.168.15.1 255.255.255.0
MLSM2(config-if)#exit
MLSM2(config)#int vlan 25
MLSM2(config-if)#ip address 192.168.25.1 255.255.255.0
MLSM2(config-if)#exit
MLSM2(config)#do write
```

#### T3
```bash
MLST3(config)#int vlan 35
MLST3(config-if)#ip address 192.168.35.1 255.255.255.0
MLST3(config-if)#exit
MLST3(config)#int vlan 15
MLST3(config-if)#ip address 192.168.45.1 255.255.255.0
MLST3(config-if)#exit
MLST3(config)#do write
```

#### T9
```bash
MLST9(config)#int vlan 35
MLST9(config-if)#ip address 192.168.55.1 255.255.255.0
MLST9(config-if)#exit
MLST9(config)#int vlan 25
MLST9(config-if)#ip address 192.168.65.1 255.255.255.0
MLST9(config-if)#exit
MLST9(config)#do write
```

#### Biblioteca Central
```bash
MLSBCENTRAL(config)#int vlan 35
MLSBCENTRAL(config-if)#ip address 192.168.75.1 255.255.255.0
MLSBCENTRAL(config-if)#exit
MLSBCENTRAL(config)#int vlan 25
MLSBCENTRAL(config-if)#ip address 192.168.85.1 255.255.255.0
MLSBCENTRAL(config-if)#exit
MLSBCENTRAL(config)#do write
```

### Habilitar Inter-VLAN en Switch MultiLayer

#### M2, T3, T9 y Biblioteca Central
```bash
MLSM2(config)#ip routing
MLSM2(config)#do write
```

### Configurar Static Routing en Switch MultiLayer

#### M2
```bash
MLSM2(config)#int f0/2
MLSM2(config-if)#no switchport
MLSM2(config-if)#ip address 11.0.0.2 255.255.255.0
MLSM2(config-if)#exit
MLSM2(config)#ip route 192.168.35.0 255.255.255.0 11.0.0.3
MLSM2(config)#ip route 192.168.45.0 255.255.255.0 11.0.0.3
MLSM2(config)#ip route 192.168.55.0 255.255.255.0 11.0.0.3
MLSM2(config)#ip route 192.168.75.0 255.255.255.0 11.0.0.3
MLSM2(config)#do write
```

#### T3
```bash
MLST3(config)#int f0/2
MLST3(config-if)#no switchport
MLST3(config-if)#ip address 11.0.0.3 255.255.255.0
MLST3(config-if)#exit
MLST3(config)#int f0/6
MLST3(config-if)#no switchport
MLST3(config-if)#ip address 13.0.0.3 255.255.255.0
MLST3(config-if)#exit
MLST3(config)#int f0/10
MLST3(config-if)#no switchport
MLST3(config-if)#ip address 12.0.0.3 255.255.255.0
MLST3(config-if)#exit
MLST3(config)#ip route 192.168.15.0 255.255.255.0 11.0.0.2
MLST3(config)#ip route 192.168.25.0 255.255.255.0 11.0.0.2
MLST3(config)#ip route 192.168.55.0 255.255.255.0 12.0.0.2
MLST3(config)#ip route 192.168.65.0 255.255.255.0 12.0.0.2
MLST3(config)#ip route 192.168.75.0 255.255.255.0 13.0.0.2
MLST3(config)#ip route 192.168.85.0 255.255.255.0 13.0.0.2
MLST3(config)#do write
```

#### T9
```bash
MLST9(config)#int f0/2
MLST9(config-if)#no switchport
MLST9(config-if)#ip address 12.0.0.2 255.255.255.0
MLST9(config-if)#exit
MLST9(config)#ip route 192.168.35.0 255.255.255.0 12.0.0.3
MLST9(config)#ip route 192.168.45.0 255.255.255.0 12.0.0.3
MLST9(config)#ip route 192.168.15.0 255.255.255.0 12.0.0.3
MLST9(config)#ip route 192.168.75.0 255.255.255.0 12.0.0.3
MLST9(config)#do write
```

#### Biblioteca Central
```bash
MLSBCENTRAL(config)#int f0/2
MLSBCENTRAL(config-if)#no switchport
MLSBCENTRAL(config-if)#ip address 13.0.0.2 255.255.255.0
MLSBCENTRAL(config-if)#exit
MLSBCENTRAL(config)#ip route 192.168.35.0 255.255.255.0 13.0.0.3
MLSBCENTRAL(config)#ip route 192.168.45.0 255.255.255.0 13.0.0.3
MLSBCENTRAL(config)#ip route 192.168.15.0 255.255.255.0 13.0.0.3
MLSBCENTRAL(config)#ip route 192.168.55.0 255.255.255.0 13.0.0.3
MLSBCENTRAL(config)#do write
```

### Configurar Listas de Control de Acceso (ACL)

#### M2, T9 y Biblioteca Central
```bash
MLSM2(config)#ip access-list extended ACL_VISITANTES
MLSM2(config-ext-nacl)#permit icmp any 192.168.45.0 0.0.0.255 echo-reply # Permitir respuestas ICMP a SOPORTE unicamente
MLSM2(config-ext-nacl)#exit

MLSM2(config)#ip access-list extended ACL_RECURSOS
MLSM2(config-ext-nacl)#permit icmp any 192.168.45.0 0.0.0.255 echo-reply # Permitir respuestas ICMP a SOPORTE
MLSM2(config-ext-nacl)#deny icmp any 192.168.45.0 0.0.0.255 echo # Denegar peticion ICMP a SOPORTE
MLSM2(config-ext-nacl)#permit icmp any any echo # Permitir tráfico ICMP (ping)
MLSM2(config-ext-nacl)#permit icmp any any echo-reply # Permitir respuestas de ICMP (ping)
MLSM2(config-ext-nacl)#exit

MLSM2(config)#int vlan 25
MLSM2(config-if)#ip access-group ACL_VISITANTES in
MLSM2(config-if)#int vlan 35
MLSM2(config-if)#ip access-group ACL_RECURSOS in
MLSM2(config-if)#exit
MLSM2(config)#do write
```

#### T3
```bash
MLST3(config)#ip access-list extended ACL_SOPORTE
MLST3(config-ext-nacl)#permit icmp any any echo
MLST3(config-ext-nacl)#permit icmp any any echo-reply
MLST3(config-ext-nacl)#exit

MLST3(config)#ip access-list extended ACL_RECURSOS
MLST3(config-ext-nacl)#permit icmp any 192.168.45.0 0.0.0.255 echo-reply
MLST3(config-ext-nacl)#deny icmp any 192.168.45.0 0.0.0.255 echo
MLST3(config-ext-nacl)#permit icmp any any echo
MLST3(config-ext-nacl)#permit icmp any any echo-reply
MLST3(config-ext-nacl)#exit

MLST3(config)#int vlan 15
MLST3(config-if)#ip access-group ACL_SOPORTE out
MLST3(config-if)#int vlan 35
MLST3(config-if)#ip access-group ACL_RECURSOS in
MLST3(config-if)#exit
MLST3(config)#do write
```



### Configurar LACP

#### M2, T9 y Biblioteca Central

Ya que se configuro una única interfaz hasta este punto, vamos a desconfigurarla para configurar la dirección IP en el port-channel.

M2: 11.0.0.2
T9: 12.0.0.2 
Biblioteca central: 13.0.0.2

```bash
MLSM2(config)#int f0/2
MLSM2(config-if)#no ip address

MLSM2(config)#int range f0/2-5
MLSM2(config-if-range)#no switchport
MLSM2(config-if-range)#channel-group 1 mode active
MLSM2(config-if-range)#channel-protocol lacp
MLSM2(config-if-range)#interface Port-channel1
MLSM2(config-if)#ip address <ip salida> 255.255.255.0
MLSM2(config-if)# exit
MLSM2(config)# do write
```

#### T3

```bash
MLST3(config)#int f0/2
MLST3(config-if)#no ip address
MLST3(config-if)#int f0/6
MLST3(config-if)#no ip address
MLST3(config-if)#int f0/10
MLST3(config-if)#no ip address
MLST3(config)#exit

MLST3(config)#int range f0/2-5
MLSM2(config-if-range)#no switchport
MLSM2(config-if-range)#channel-group 1 mode active
MLSM2(config-if-range)#channel-protocol lacp
MLSM2(config-if-range)#interface Port-channel1
MLSM2(config-if)#ip address 11.0.0.3 255.255.255.0
MLSM2(config-if)#exit

MLST3(config)#int range f0/6-9
MLSM2(config-if-range)#no switchport
MLSM2(config-if-range)#channel-group 2 mode active
MLSM2(config-if-range)#channel-protocol lacp
MLSM2(config-if-range)#interface Port-channel2
MLSM2(config-if)#ip address 13.0.0.3 255.255.255.0
MLSM2(config-if)#exit

MLST3(config)#int range f0/10-13
MLSM2(config-if-range)#no switchport
MLSM2(config-if-range)#channel-group 3 mode active
MLSM2(config-if-range)#channel-protocol lacp
MLSM2(config-if-range)#interface Port-channel3
MLSM2(config-if)#ip address 12.0.0.3 255.255.255.0
MLSM2(config-if)#exit

MLST3(config)#do write
```

### Configurar OSPF

Para ver que puertos o interfaces, incluso redes que indican la letra C, que estan listos para conectarse
se ingresa el comando 
```bash
show ip route
```

Para ver que configuraciones de OSPF hay que agregar
#### Para el switch T3
```bash
router ospf 1
router-id 3.3.3.3           
network 11.0.0.0 0.0.0.255 area 23  
network 12.0.0.0 0.0.0.255 area 23  
network 13.0.0.0 0.0.0.255 area 23 
network 192.168.35.0 0.0.0.255 area 23
network 192.168.45.0 0.0.0.255 area 23
```
Se configura los puertos con el md5 de autenticación
```bash
interface Port-channel1
ip ospf message-digest-key 23 md5 redes2-g23  
ip ospf authentication message-digest
exit
interface Port-channel2
ip ospf message-digest-key 23 md5 redes2-g23
ip ospf authentication message-digest
exit
interface Port-channel3
ip ospf message-digest-key 23 md5 redes2-g23
ip ospf authentication message-digest
exit
```
#### Para el Switch M2
```bash
router ospf 1
router-id 2.2.2.2          
network 11.0.0.0 0.0.0.255 area 23  
network 192.168.15.0 0.0.0.255 area 23  
network 192.168.25.0 0.0.0.255 area 23 
exit
interface Port-channel1
ip ospf message-digest-key 23 md5 redes2-g23  
ip ospf authentication message-digest
exit
```

#### Para el Switch T9
```bash
router ospf 1
router-id 9.9.9.9          
network 12.0.0.0 0.0.0.255 area 23  
network 192.168.55.0 0.0.0.255 area 23  
network 192.168.65.0 0.0.0.255 area 23 
exit
interface Port-channel1
ip ospf message-digest-key 23 md5 redes2-g23  
ip ospf authentication message-digest
exit
```


#### Para el Switch Biblioteca Central
```bash
router ospf 1
router-id 1.1.1.1         
network 13.0.0.0 0.0.0.255 area 23  
network 192.168.75.0 0.0.0.255 area 23  
network 192.168.85.0 0.0.0.255 area 23 
exit
interface Port-channel1
ip ospf message-digest-key 23 md5 redes2-g23  
ip ospf authentication message-digest
exit
```
Para observar las configuraciones realizadas se ingresa el comando y mostrar sus resultados

```bash
show ip ospf neighbor
```



### Configurar EIGRP

#### Multilayer Switch 1

```bash
MLSM2(config)#router eigrp 23
MLSM2(config-router)#network 11.0.0.2 0.0.0.255
MLSM2(config-router)#passive-interface f0/1
MLSM2(config-router)#no auto-summary
MLSM2(config-router)#end
```

#### Multilayer Switch 2

```bash
MLST3(config)#router eigrp 23
MLST3(config-router)#network 11.0.0.3 0.0.0.255
MLST3(config-router)#network 12.0.0.3 0.0.0.255
MLST3(config-router)#network 13.0.0.3 0.0.0.255
MLST3(config-router)#passive-interface f0/1
MLST3(config-router)#no auto-summary
MLST3(config-router)#end
```

#### Multilayer Switch 3

```bash
MLST9(config)#router eigrp 23
MLST9(config-router)#network 12.0.0.3 0.0.0.255
MLST9(config-router)#passive-interface f0/1
MLST9(config-router)#no auto-summary
MLST9(config-router)#end
```

#### Multilayer Switch 0

```bash
MLSBCENTRAL(config)#router eigrp 23
MLSBCENTRAL(config-router)#network 13.0.0.3 0.0.0.255
MLSBCENTRAL(config-router)#passive-interface f0/1
MLSBCENTRAL(config-router)#no auto-summary
MLSBCENTRAL(config-router)#end
```

#### Verificar el Routing EIGRP

```bash
MLSBCENTRAL#show ip eigrp neighbors
MLSBCENTRAL#show ip protocols
```

### Notas

#### Mostrar listas de acceso
```bash
do show access-lists
```

#### Mostrar listas de acceso asignadas en interfaces
```bash
do sh run | I interface| access-group
```

#### Eliminar lista de acceso
```bash
no ip access-list extended ACL_RECURSOS
```

#### Eliminar lista de acceso asignada en interfaz
```bash
int vlan 35
no ip access-group ACL_RECURSOS <in/out (depende de como se creo la lista de acceso)>
```

#### Listas canales EtherChannel (LACP)
```bash
do show etherchannel summary
```







1. configure LACP in MS 1, 2, 3 and 7, add a power supply to 3650 switches. `interface range [type(f/g)] [range(example: 0/1-4)]`, select protocol `channel-protocol pagp/lacp`, select group and mode, you can create groups only 
from 1-6. For LACP `channel-group 1 mode active`

3. configure vtp. We will have two servers on each side, only access and routing layer cares about VLANs, the core layer doesn't configure any VLAN set up, but also be aware "vtp" protocol can not travel throuhg layer 3 devices, such as swichports acting as routers or a router itself, that is why we will have two servers, one on each side. MS6 on the left side and MS10 on the right side. `vtp mode server`, `vtp version 2`, `vtp domain juan`, `vtp password juan`. Now we will configure the clients on each side. Left side clients are MS4, MS5, SW0 and S1. Right side clients are MS8, MS9, SW2 and SW3. Do `vtp mode client`, `vtp domain juan`, `vtp password juan`

4. Create vlans on server switches MS6 and MS10. `vlan 10`, `name orange`, `vlan 20`, `name green`

5. Configure Access connections. This is only done in layer 2 switches interfaces connected to end devices and they are considered the Access layer. On the left side access layer is SW0 and SW1, on ports connected to end devices run `int fa0/#`, `sw mode access`, `sw access vlan [vlan#]`

6. Configure trunk connections. This is done in connections between access and routing layer, and withing the routing layer itself. Access layer on the left side is represented by end devices and the connections between 









