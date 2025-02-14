# PRACTICA 1 - REDES DE COMPUTADORAS 2
## Integrantes
| Nombre   | Carnet    |
|----------|----------|
| Diego Andres Huite Alvarez  | 202003585  |
| Gerhard Benjamin Ardon Valdez  | 202004796  |
| Pablo Javier Batz Contreras  | 201902698  |
| José Manuel Lacán Chavajay   | 201900087  |

# Configuración basica Switch
```
enable
configure terminal
no ip domain-lookup
hostname SW0_G2
enable secret redes2grupo2 (Solo en Switch0 y Switch11)

```

# Configuración modo troncal 

```
enable
configure terminal
interface range Fa0/1-n
switchport mode trunk
switchport trunk allowed vlan all
do wr

```

## Configuración VTP, Configuración Servidor
### Para Switch0 y Switch11

```
enable
configure terminal
vtp mode server
vtp domain g2
vtp password redes2grupo2
vtp version 2
do wr
show vtp status
```
# Configuración Modo Cliente
## Para todos los Switches faltantes 
```
enable
configure terminal
vtp mode client
vtp domain g2
vtp password redes2grupo2
do wr
show vtp status

```
## Configuración VLAN solo en Switches Servidores:  Switch0 y Switch11

### Edificio: Izquierdo
```
enable
configure terminal
vlan 12
name Primaria
vlan 22
name Basicos
vlan 32
name Diversificado
```

## 

## Edificio: Derecho
```
vlan 42
name Primaria
vlan 52
name Basicos
vlan 62
name Diversificado
do wr
```
## Configuracion Acces moede Para las interfaces conectadas a las PC

```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 12 (para primaria)
do wr
show interface status
```

### Modo Acceso Izquierda Para las interfaces conectadas a las PC
## Switch 6 y 7
```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 12 (para primaria)
do wr

```

## Switch 9 Y 10
```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 32 (para diversificado)
do wr
```


## Modo acceso Derecha (Para las interfaces conectadas a las PC)
## Switch 17 y 18
```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 42 (para primaria)
do wr
```


## Switch 19
```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 62 (para diversificado)
do wr

```


## Switch 20 Y 21

```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 52 (para básicos)
do wr
```
# Enrutamiento 
## Configuracion Basica Router

```
enable
configure terminal
no ip domain-lookup
hostname R0_G2
interface Gig0/1
no shutdown
```

# Configuración IP routers (Solo en router0 y router1)
## Router0
```
Router-on-a-stick
Subinterfaz
enable
configure terminal
interface Gig0/1.12
encapsulation dot1q 12
ip address 192.168.12.1 255.255.255.0
exit
interface Gig0/1.22
encapsulation dot1q 22
ip address 192.168.22.1 255.255.255.0
exit
interface Gig0/1.32
encapsulation dot1q 32
ip address 192.168.32.1 255.255.255.0
exit
interface Gig0/0
ip address 10.0.3.1 255.255.255.0
no shutdown 
do wr
```
# Router1
```
Router-on-a-stick
Subinterfaz
enable
configure terminal
interface Gig0/1.42
encapsulation dot1q 42
ip address 192.168.42.1 255.255.255.0
exit
interface Gig0/1.52
encapsulation dot1q 52
ip address 192.168.52.1 255.255.255.0
exit
interface Gig0/1.62
encapsulation dot1q 62
ip address 192.168.62.1 255.255.255.0
exit
interface Gig0/0
ip address 10.0.5.1 255.255.255.0
no shutdown 
do wr

```
# router2
```
interface Gig0/0
ip address 10.0.3.2 255.255.255.0
no shutdown
interface Gig0/1
ip address 10.0.4.1 255.255.255.0
no shutdown
do wr
```
# router3 
```
interface Gig0/0
ip address 10.0.5.2 255.255.255.0
no shutdown
interface Gig0/1
ip address 10.0.4.2 255.255.255.0
no shutdown
do wr
```

# ENRUTAMIENTO DINAMICO OSPF (Router0 y Router2)

## router0
```
router ospf 10
network 192.168.12.0 0.0.0.255 área 0
network 192.168.22.0 0.0.0.255 área 0
network 192.168.32.0 0.0.0.255 área 0
network 10.0.3.0 0.0.0.255 área 0
```

#  EIGRP (Router1 y Router3) router1

```
router eigrp 10
network 192.168.42.0 0.0.0.255
network 192.168.52.0 0.0.0.255
network 192.168.62.0 0.0.0.255
network 10.0.5.0 0.0.0.255
no auto-summary

```

# router3
```
router eigrp 10
network 10.0.5.0 0.0.0.255
no auto-summary
redistribute rip metric 25600 1010 255 255 1500
```

# RIP (Router2 y Router3) router2

```
router rip
versión 2
network 10.0.4.0
redistribute ospf 10 metric 2
```

# router3
```
router rip
versión 2
network 10.0.4.0
redistribute eigrp 10 metric 2
```
# TOPOLOGIA DE RED

![image](https://github.com/user-attachments/assets/63acb9a8-175c-4348-8e78-468803b97497)


---
# SEGURIDAD
La configuración aplicada a cada switch en modo acceso fue la siguiente:
![image](https://github.com/user-attachments/assets/29f9fa1a-fdf4-4bdb-96a3-298b1b6559d0)


```console
interface FastEthernet0/11 
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address sticky
```
