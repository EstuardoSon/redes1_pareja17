### **Universidad San Carlos de Guatemala**
### **Facultad de Ingeniería**
### **Escuela de Ciencias y Sistemas**
### **Redes de Computadoras 1 N**
### **Catedrático: Ing. Pedro Pablo Hernández Ramírez**
### **Auxiliar: Edwin López**

## **Manual Técnico - Práctica 2**

- **Estuardo Gabriel Son Mux – 202003894**
- **Angel Eduardo Marroquín Canizales – 202003959**

## **Direcciones IP utilizadas**
| Dispositivo | Interfaz | IP | 
| ----------- | -------- | -- |
| Central | s1/0 | 10.0.0.1 |
| Central | e0/0 | 117.168.1.2 |
| Central | e0/1 | 117.168.2.2 |
| R2 | e0/0 | 117.168.1.1 |
| R2 | e0/1 | 117.168.0.2 |
| R3 | e0/0 | 117.168.2.1 |
| R3 | e0/1 | 117.168.0.3 |
| R2-R3 | Vitual | 117.168.0.1 |
| VPC11 | eth0 | 117.168.0.4 |
| Villa_Nueva | s1/0 | 10.0.0.2 |
| Villa_Nueva | e0/0 | 117.178.1.1 |
| Villa_Nueva | e0/1 | 117.178.2.1 |
| R5 | e0/0 | 117.178.1.2 |
| R5 | e0/1 | 117.178.0.2 |
| R6 | e0/0 | 117.178.2.2 |
| R6 | e0/1 | 117.178.0.3 |
| R5-R6 | Virtual | 117.178.0.1 |
| VPC12 | eth0 | 117.178.0.4 |

## **Rrangos de IPs disponibles**
| ID de red | Máscara de subred | Direcciones disponibles | Primera IP | última IP | Dirección de broadcast |
| --------- | ----------------- | ----------------------- | ---------- | --------- | -- |
| 10.0.0.0 | 255.255.255.252 | 2 | 10.0.0.1 | 10.0.0.2 | 10.0.0.3 |
| 117.168.0.0 | 255.255.255.0 | 254 | 117.168.0.1 | 117.168.0.254 | 117.168.0.255 |
| 117.168.1.0 | 255.255.255.248 | 6 | 117.168.1.1 | 117.168.1.6 | 117.168.1.7 |
| 117.168.2.0 | 255.255.255.248 | 6 | 117.168.2.1 | 117.168.2.6 | 117.168.2.7 |
| 117.178.1.0 | 255.255.255.248 | 6 | 117.178.1.1 | 117.178.1.6 | 117.178.1.7 |
| 117.178.2.0 | 255.255.255.248 | 6 | 117.178.2.1 | 117.178.2.6 | 117.178.2.7 |
| 117.178.0.0 | 255.255.255.0 | 254 | 117.178.0.1 | 117.178.0.254 | 117.178.0.255 | 

## **Configuraciones**

### **Central**
```CMD
interface e0/0
ip address 117.168.1.2 255.255.255.248
no shutdown
exit

interface e0/1
ip address 117.168.2.2 255.255.255.248
no shutdown
exit

interface s1/0
ip address 10.0.0.1 255.255.255.252
no shutdown
exit

ip route 117.168.1.0 255.255.255.248 117.168.1.1
ip route 117.168.2.0 255.255.255.248 117.168.2.1
ip route 117.168.0.0 255.255.255.0 117.168.1.1
ip route 117.168.0.0 255.255.255.0 117.168.2.1

ip route 10.0.0.0 255.255.255.252 10.0.0.2

ip route 117.178.1.0 255.255.255.248 10.0.0.2
ip route 117.178.2.0 255.255.255.248 10.0.0.2
ip route 117.178.0.0 255.255.255.0 10.0.0.2
```
### **R2**
```CMD
interface e0/1
ip address 117.168.0.2 255.255.255.0

! glbp
glbp 1 ip 117.168.0.1
glbp 1 preempt
glbp 1 priority 209
glbp 1 load-balancing round-robin
no shutdown
exit

interface e0/0
ip address 117.168.1.1 255.255.255.248
no shutdown
exit

ip route 117.168.1.0 255.255.255.248 117.168.1.2
ip route 10.0.0.0 255.255.255.252 117.168.1.2
ip route 117.178.1.0 255.255.255.248 117.168.1.2
ip route 117.178.2.0 255.255.255.248 117.168.1.2
ip route 117.178.0.0 255.255.255.0 117.168.1.2

ip route 117.168.2.0 255.255.255.248 117.168.0.3
! ip route 10.0.0.0 255.255.255.252 117.168.0.3
! ip route 117.178.1.0 255.255.255.248 117.168.0.3
! ip route 117.178.2.0 255.255.255.248 117.168.0.3
! ip route 117.178.0.0 255.255.255.0 117.168.0.3
```

### **R5**
```CMD
interface e0/1
ip address 117.178.0.2 255.255.255.0

! hsrp
standby version 2
standby 2 ip 117.178.0.1
standby 2 preempt
standby 2 priority 209
no shutdown
exit

interface e0/0
ip address 117.178.1.2 255.255.255.248
no shutdown
exit

ip route 117.178.1.0 255.255.255.248 117.178.1.1
ip route 10.0.0.0 255.255.255.252 117.178.1.1
ip route 117.168.1.0 255.255.255.248 117.178.1.1
ip route 117.168.2.0 255.255.255.248 117.178.1.1
ip route 117.168.0.0 255.255.255.0 117.178.1.1

ip route 117.178.2.0 255.255.255.248 117.178.0.3
! ip route 10.0.0.0 255.255.255.252 117.178.0.3
! ip route 117.168.1.0 255.255.255.248 117.178.0.3
! ip route 117.168.2.0 255.255.255.248 117.178.0.3
! ip route 117.168.0.0 255.255.255.0 117.178.0.3
```

### **SW7**

```CMD
interface Port-channel 1
description SW8SW7
exit
interface range ethernet 0/2-3
channel-group 1 mode passive
switchport trunk encapsulation dot1q
switchport mode trunk
exit
```

### **VPC11**

```CMD
ip 117.168.0.4 117.168.0.1 24
```

### **Comandos usados**

**Ruta estática**
```CMD
ip route [red de destino] [máscara de subred] [siguiente salto]
ip route 117.168.1.0 255.255.255.248 117.168.1.2
```
**PortChannel con PAGP**  
SW9
```CMD
interface Port-channel 1
description SW9SW10
exit
interface range ethernet 0/2-3 
channel-group 1 mode auto
```
SW10
```CMD
interface Port-channel 1
description SW9SW10
exit
interface range ethernet 0/0-1
channel-group 1 mode desirable
```
**PortChannel con LACP**  
SW8
```CMD
interface Port-channel 1
description SW8SW7
exit
interface range ethernet 0/0-1
channel-group 1 mode active
```
SW7
```CMD
interface Port-channel 1
description SW8SW7
exit
interface range ethernet 0/2-3
channel-group 1 mode passive
```
**HSRP**  
R5
```CMD
! hsrp
standby version 2
standby 2 ip 117.178.0.1
standby 2 preempt
standby 2 priority 209
no shutdown
```
R6
```CMD
! hsrp
standby version 2
standby 2 ip 117.178.0.1
no shutdown
```
**GLBP**  
R2
```CMD
! glbp
glbp 1 ip 117.168.0.1
glbp 1 preempt
glbp 1 priority 209
glbp 1 load-balancing round-robin
no shutdown
```
R3
```CMD
! glbp 
glbp 1 ip 117.168.0.1
glbp 1 load-balancing round-robin
no shutdown
```
**VPC**
```CMD
ip 117.168.0.4 117.168.0.1 24
```

## **Comandos empleados para la verificación**
```CMD
! PortChannel
show etherchannel summary
show pagp neighbor
show lacp neighbor

! Rutas estáticas
show ip route
show running-config | section ip route

! HSRP
show standby
show standby brief

! GLBP
show glbp brief
```

## **Resto de Configuraciones**

## SW8
```CMD
interface Port-channel 1
description SW8SW7
exit
interface range ethernet 0/0-1
channel-group 1 mode active
switchport trunk encaptulation dot1q
exit

interface e0/2
switchport mode access
exit
```

## SW7

```CMD
interface Port-channel 1
description SW8SW7
exit
interface range ethernet 0/2-3
channel-group 1 mode passive
switchport trunk encapsulation dot1q
switchport mode trunk
exit
```

## SW9
```CMD
interface Port-channel 1
description SW9SW10
exit
interface range ethernet 0/2-3 
channel-group 1 mode auto
switchport trunk encapsulation dot1q
switchport mode trunk
exit
```

## SW10
```CMD
interface Port-channel 1
description SW9SW10
exit
interface range ethernet 0/0-1
channel-group 1 mode desirable
switchport trunk encapsulation dot1q
switchport mode trunk
exit

interface e0/2
switchport mode access
exit
```

## R2
```CMD
interface e0/1
ip address 117.168.0.2 255.255.255.0

! glbp
glbp 1 ip 117.168.0.1
glbp 1 preempt
glbp 1 priority 209
glbp 1 load-balancing round-robin
no shutdown
exit

interface e0/0
ip address 117.168.1.1 255.255.255.248
no shutdown
exit

ip route 117.168.1.0 255.255.255.248 117.168.1.2
ip route 10.0.0.0 255.255.255.252 117.168.1.2
ip route 117.178.1.0 255.255.255.248 117.168.1.2
ip route 117.178.2.0 255.255.255.248 117.168.1.2
ip route 117.178.0.0 255.255.255.0 117.168.1.2

ip route 117.168.2.0 255.255.255.248 117.168.0.3
! ip route 10.0.0.0 255.255.255.252 117.168.0.3
! ip route 117.178.1.0 255.255.255.248 117.168.0.3
! ip route 117.178.2.0 255.255.255.248 117.168.0.3
! ip route 117.178.0.0 255.255.255.0 117.168.0.3
```

## R3
```CMD
interface e0/1
ip address 117.168.0.3 255.255.255.0

! glbp 
glbp 1 ip 117.168.0.1
glbp 1 load-balancing round-robin
no shutdown
exit

interface e0/0
ip address 117.168.2.1 255.255.255.248
no shutdown
exit

ip route 117.168.2.0 255.255.255.248 117.168.2.2
ip route 10.0.0.0 255.255.255.252 117.168.2.2
ip route 117.178.1.0 255.255.255.248 117.168.2.2
ip route 117.178.2.0 255.255.255.248 117.168.2.2
ip route 117.178.0.0 255.255.255.0 117.168.2.2

ip route 117.168.1.0 255.255.255.248 117.168.0.2
! ip route 10.0.0.0 255.255.255.252 117.168.0.2
! ip route 117.178.1.0 255.255.255.248 117.168.0.2
! ip route 117.178.2.0 255.255.255.248 117.168.0.2
! ip route 117.178.0.0 255.255.255.0 117.168.0.2
```

## R5
```CMD
interface e0/1
ip address 117.178.0.2 255.255.255.0

! hsrp
standby version 2
standby 2 ip 117.178.0.1
standby 2 preempt
standby 2 priority 209
no shutdown
exit

interface e0/0
ip address 117.178.1.2 255.255.255.248
no shutdown
exit

ip route 117.178.1.0 255.255.255.248 117.178.1.1
ip route 10.0.0.0 255.255.255.252 117.178.1.1
ip route 117.168.1.0 255.255.255.248 117.178.1.1
ip route 117.168.2.0 255.255.255.248 117.178.1.1
ip route 117.168.0.0 255.255.255.0 117.178.1.1

ip route 117.178.2.0 255.255.255.248 117.178.0.3
! ip route 10.0.0.0 255.255.255.252 117.178.0.3
! ip route 117.168.1.0 255.255.255.248 117.178.0.3
! ip route 117.168.2.0 255.255.255.248 117.178.0.3
! ip route 117.168.0.0 255.255.255.0 117.178.0.3
```

## R6
```CMD
interface e0/1
ip address 117.178.0.3 255.255.255.0

! hsrp
standby version 2
standby 2 ip 117.178.0.1
no shutdown
exit

interface e0/0
ip address 117.178.2.2 255.255.255.248
no shutdown
exit

ip route 117.178.2.0 255.255.255.248 117.178.2.1
ip route 10.0.0.0 255.255.255.252 117.178.2.1
ip route 117.168.1.0 255.255.255.248 117.178.2.1
ip route 117.168.2.0 255.255.255.248 117.178.2.1
ip route 117.168.0.0 255.255.255.0 117.178.2.1

ip route 117.178.1.0 255.255.255.248 117.178.0.2
! ip route 10.0.0.0 255.255.255.252 117.178.0.2
! ip route 117.168.1.0 255.255.255.248 117.178.0.2
! ip route 117.168.2.0 255.255.255.248 117.178.0.2
! ip route 117.168.0.0 255.255.255.0 117.178.0.2
```

## Central
```CMD
interface e0/0
ip address 117.168.1.2 255.255.255.248
no shutdown
exit

interface e0/1
ip address 117.168.2.2 255.255.255.248
no shutdown
exit

interface s1/0
ip address 10.0.0.1 255.255.255.252
no shutdown
exit

ip route 117.168.1.0 255.255.255.248 117.168.1.1
ip route 117.168.2.0 255.255.255.248 117.168.2.1
ip route 117.168.0.0 255.255.255.0 117.168.1.1
ip route 117.168.0.0 255.255.255.0 117.168.2.1

ip route 10.0.0.0 255.255.255.252 10.0.0.2

ip route 117.178.1.0 255.255.255.248 10.0.0.2
ip route 117.178.2.0 255.255.255.248 10.0.0.2
ip route 117.178.0.0 255.255.255.0 10.0.0.2
```

## Villa_Nueva
```CMD
interface e0/0
ip address 117.178.1.1 255.255.255.248
no shutdown
exit

interface e0/1
ip address 117.178.2.1 255.255.255.248
no shutdown
exit

interface s1/0
ip address 10.0.0.2 255.255.255.252
no shutdown
exit

ip route 117.178.1.0 255.255.255.248 117.178.1.2
ip route 117.178.2.0 255.255.255.248 117.178.2.2
ip route 117.178.0.0 255.255.255.0 117.178.1.2
ip route 117.178.0.0 255.255.255.0 117.178.2.2

ip route 10.0.0.0 255.255.255.252 10.0.0.1

ip route 117.168.1.0 255.255.255.248 10.0.0.1
ip route 117.168.2.0 255.255.255.248 10.0.0.1
ip route 117.168.0.0 255.255.255.0 10.0.0.1
```