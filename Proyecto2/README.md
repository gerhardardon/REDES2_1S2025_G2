# Proyecto 2

## Topología propuesta

## Comandos utilizados
```
Interconexión de ISPs

ISP1
hostname ISP1
ip routing
int gig1/1/1
no switchport
ip address 17.2.0.1 255.255.255.252
int gig1/1/2
no switchport
ip address 17.2.0.5 255.255.255.252
router bgp 1
neighbor 17.2.0.2 remote-as 2
neighbor 17.2.0.6 remote-as 3
network 17.2.0.0 mask 255.255.255.252
network 17.2.0.4 mask 255.255.255.252
redistribute eigrp 2
int gig1/0/1
no switchport
ip address 192.168.12.161 255.255.255.252
router eigrp 2
network 192.168.12.160 0.0.0.3
redistribute bgp 1 metric 100 1 255 1 1500


ISP2
hostname ISP2
ip routing
int gig1/1/1
no switchport 
ip address 17.2.0.2 255.255.255.252
int gig1/1/3
no switchport
ip address 17.2.0.9 255.255.255.252
router bgp 2
neighbor 17.2.0.1 remote-as 1
neighbor 17.2.0.10 remote-as 3
network 17.2.0.0 mask 255.255.255.252
network 17.2.0.8 mask 255.255.255.252
redistribute eigrp 3
int gig1/0/1
no switchport
ip address 192.168.22.181 255.255.255.252
router eigrp 3
network 192.168.22.180 0.0.0.3
redistribute bgp 2 metric 100 1 255 1 1500

ISP3
hostname ISP3
ip routing
int gig1/1/2
no switchport
ip address 17.2.0.6 255.255.255.252
int gig1/1/3
no switchport 
ip address 17.2.0.10 255.255.255.252
router bgp 3
neighbor 17.2.0.5 remote-as 1
neighbor 17.2.0.9 remote-as 2
network 17.2.0.8 mask 255.255.255.252
network 17.2.0.4 mask 255.255.255.252
redistribute ospf 2 
int gig1/0/1
no switchport
ip address 192.168.32.201 255.255.255.252
router ospf 2
network 192.168.32.200 0.0.0.3 area 0
redistribute bgp 3 subnets

ISP 1: Telecom Uno
Switches 2960
vlan 2
name Administración (celeste)
exit
vlan 3
name Atencion_al_cliente (verde)
S1
hostname S1
int fa0/11
switchport mode access
switchport access vlan 2
int fa0/12
switchport mode access
switchport access vlan 3
int range gig0/1-2
switchport mode trunk
switchport trunk allowed vlan 2,3

S2
int fa0/1
switchport mode trunk
switchport trunk allowed vlan 2
int fa0/11
switchport mode access
switchport access vlan 2

S3
int gig0/1
switchport mode trunk
switchport trunk allowed vlan 2,3
int fa0/12
switchport mode access
switchport access vlan 2
int fa0/11
switchport mode access
switchport access vlan 3


MSW1
ip routing
int range fa0/1-3
channel-group 1 mode active
int port-channel 1
no switchport 
ip address 192.168.12.165 255.255.255.252
hostName MSW1
int range fa0/4-6
channel-group 2 mode active
int port-channel 2
no switchport
ip address 192.168.12.169 255.255.255.252
int fa0/7
no switchport
ip address 192.168.12.162 255.255.255.252
int gig0/1
no switchport
ip address 192.168.12.173 255.255.255.252
int gig0/2
no switchport
ip address 192.168.12.177 255.255.255.252
router eigrp 2
network 192.168.12.160 0.0.0.3
network 192.168.12.164 0.0.0.3
network 192.168.12.168 0.0.0.3
network 192.168.12.172 0.0.0.3
network 192.168.12.176 0.0.0.3

MSW2
ip routing
int port-channel 1
no switchport
ip address 192.168.12.166 255.255.255.252
hostname MSW2
int vlan 2
ip address 192.168.12.1 255.255.255.224
ip helper-address 192.168.12.182
vlan 2
name Administración
int fa0/11
no switchport
ip address 192.168.12.181 255.255.255.252
router eigrp 2
network 192.168.12.0 0.0.0.31
network 192.168.12.164 0.0.0.3
network 192.168.12.180 0.0.0.3
access-list 100 permit icmp 192.168.2.64 0.0.0.31 192.168.2.0 0.0.0.31 echo


MSW3
ip routing
int range fa0/4-6
channel-group 2 mode active
int port-channel 2
no switchport
ip address 192.168.12.170 255.255.255.252
vlan 2
name Administracion
vlan 3
name Atencion_al_cliente
int vlan 2
ip address 192.168.12.97 255.255.255.224
ip helper-address 192.168.12.182
int vlan 3
ip address 192.168.12.129 255.255.255.224
ip helper-address 192.168.12.182
ip access-group 100 in
router eigrp 2
network 192.168.12.96 0.0.0.31
network 192.168.12.128 0.0.0.31
network 192.168.12.168 0.0.0.3
access-list 100 permit icmp 192.168.12.96 0.0.0.31 192.168.12.128 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.128 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.0 0.0.0.31 192.168.12.128 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.128 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.32 0.0.0.31 192.168.12.128 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.128 0.0.0.31 192.168.12.32 0.0.0.31 echo

access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.128 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.128 0.0.0.31 192.168.22.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.128 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.128 0.0.0.31 192.168.22.96 0.0.0.31 echo

access-list 100 permit icmp 192.168.12.32 0.0.0.31 192.168.12.128 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.128 0.0.0.31 192.168.12.32 0.0.0.31 echo

access-list 100 permit ip any any


R1
int gig0/1.2
encapsulation dot1q 2
ip address 192.168.12.34 255.255.255.224
standby 1 ip 192.168.12.33 
standby 1 priority 150
standby 1 preempt
ip helper-address 192.168.12.182
int gig0/1.3
encapsulation dot1q 3
ip address 192.168.12.66 255.255.255.224
standby 2 ip 192.168.12.65
standby 2 priority 150
standby 2 preempt
ip helper-address 192.168.12.182
ip access-group 100 in
router eigrp 2
network 192.168.12.32 0.0.0.31 
network 192.168.12.64 0.0.0.31 
network 192.168.12.172 0.0.0.3 
access-list 100 permit icmp 192.168.12.96 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.0 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.32 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.12.32 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.22.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.22.96 0.0.0.31 echo
access-list 100 permit ip any any

R2
int gig0/1.2
encapsulation dot1q 2
ip address 192.168.12.35 255.255.255.224
standby 1 ip 192.168.12.33 
ip helper-address 192.168.12.182
int gig0/1.3
encapsulation dot1q 3
ip address 192.168.12.67 255.255.255.224
standby 2 ip 192.168.12.65 
ip helper-address 192.168.12.182
ip access-group 100 in
int gig0/0
no shut
ip address 192.168.12.178 255.255.255.252
router eigrp 2
network 192.168.12.32 0.0.0.31
network 192.168.12.64 0.0.0.31
network 192.168.12.176 0.0.0.3
access-list 100 permit icmp 192.168.12.96 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.0 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.32 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.12.32 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.22.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.12.64 0.0.0.31 192.168.22.96 0.0.0.31 echo
access-list 100 permit ip any any

ISP 2: Redes Nacionales

MSW4
ip routing
hostname MSW4
int range fa0/1-3
channel-group 1 mode active
int range fa0/4-6
channel-group 2 mode active
#int gig0/1
no switchport 
ip address 192.168.22.182 255.255.255.252
int port-channel 1
no switchport
ip address 192.168.22.173 255.255.255.252
int port-channel 2
no switchport
ip address 192.168.22.177 255.255.255.252
router eigrp 3
network 192.168.22.172 0.0.0.3
network 192.168.22.176 0.0.0.3
network 192.168.22.180 0.0.0.3

MSW5
ip routing
hostname MSW5
int range fa0/1-3
channel-group 1 mode active
int gig0/1
no switchport
ip address 192.168.22.161 255.255.255.252
int fa0/4
no switchport
ip address 192.168.22.165 255.255.255.252
int port-channel 1
no switchport
ip address 192.168.22.174 255.255.255.252
router eigrp 3
network 192.168.22.160 0.0.0.3
network 192.168.22.164 0.0.0.3
network 192.168.22.172 0.0.0.3

MSW6
hostname MSW6
int range fa0/4-6
channel-group 2 mode active
int port-channel 2
no switchport
ip address 192.168.22.178 255.255.255.252
int fa0/1
no switchport
ip address 192.168.22.169 255.255.255.252
router eigrp 3
network 192.168.22.176 0.0.0.3
network 192.168.22.168 0.0.0.3

MSW7
ip routing
hostname MSW7
vlan 4
name Ventas
vlan 5
name Facturación
int fa0/4
no switchport 
ip address 192.168.22.166 255.255.255.252
int vlan 4
ip address 192.168.22.33 255.255.255.224
ip helper-address 192.168.12.182
ip access-group 101 in
interface vlan 5
ip address 192.168.22.65 255.255.255.224
ip helper-address 192.168.12.182
ip access-group 100 in
router eigrp 3
network 192.168.22.32 0.0.0.31
network 192.168.22.64 0.0.0.31
network 192.168.22.164 0.0.0.3
access-list 100 permit icmp 192.168.12.96 0.0.0.31 192.168.22.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.0 0.0.0.31 192.168.22.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.32 0.0.0.31 192.168.22.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.32 0.0.0.31 echo

access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.64 0.0.0.31 192.168.12.128 0.0.0.31 echo

access-list 100 permit ip any any

access-list 101 permit icmp 192.168.12.96 0.0.0.31 192.168.22.32 0.0.0.31 echo
access-list 101 deny icmp 192.168.22.32 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 101 permit icmp 192.168.12.0 0.0.0.31 192.168.22.32 0.0.0.31 echo
access-list 101 deny icmp 192.168.22.32 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 101 permit icmp 192.168.12.32 0.0.0.31 192.168.22.32 0.0.0.31 echo
access-list 101 deny icmp 192.168.22.32 0.0.0.31 192.168.12.32 0.0.0.31 echo
access-list 101 permit ip any any

MSW8
ip routing
hostname MSW8
vlan 4
name Ventas
vlan 5
name Facturación
int fa0/1
no switchport
ip address 192.168.22.170 255.255.255.252
int vlan 4
ip address 192.168.22.129 255.255.255.252
ip helper-address 192.168.12.182
ip access-group 101 in
int vlan 5
ip address 192.168.22.97 255.255.255.252
ip helper-address 192.168.12.182
ip access-group 100 in
access-list 100 permit icmp 192.168.12.96 0.0.0.31 192.168.22.96 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.0 0.0.0.31 192.168.22.96 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 100 permit icmp 192.168.12.32 0.0.0.31 192.168.22.96 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.32 0.0.0.31 echo

access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.64 0.0.0.31 echo
access-list 100 deny icmp 192.168.22.96 0.0.0.31 192.168.12.128 0.0.0.31 echo

access-list 100 permit ip any any

access-list 101 permit icmp 192.168.12.96 0.0.0.31 192.168.22.128 0.0.0.31 echo
access-list 101 deny icmp 192.168.22.128 0.0.0.31 192.168.12.96 0.0.0.31 echo
access-list 101 permit icmp 192.168.12.0 0.0.0.31 192.168.22.128 0.0.0.31 echo
access-list 101 deny icmp 192.168.22.128 0.0.0.31 192.168.12.0 0.0.0.31 echo
access-list 101 permit icmp 192.168.12.32 0.0.0.31 192.168.22.128 0.0.0.31 echo
access-list 101 deny icmp 192.168.22.128 0.0.0.31 192.168.12.32 0.0.0.31 echo
access-list 101 permit ip any any


S4
hostname S4
vlan 4
name Ventas
vlan 5
name Facturacion
int gig0/1
switchport mode trunk
switchport trunk allowed vlan 4,5
int fa0/11
switchport mode access
switchport access vlan 4
int range fa0/12-13
switchport mode access
switchport access vlan 5

S5
hostname S5
vlan 4
name Ventas
vlan 5
name Facturacion
int gig0/1
switchport mode trunk
switchport trunk allowed vlan 4,5
int fa0/11
switchport mode access
switchport access vlan 4
int range fa0/12
switchport mode access
switchport access vlan 5

ISP 3: Conexiones Futuras

R3
hostname R3
int Se0/0/0
ip address 192.168.32.193 255.255.255.252
int Se0/0/1
ip address 192.168.32.197 255.255.255.252
int gig0/0
ip address 192.168.32.202 255.255.255.252
router ospf 2
network 192.168.32.192 0.0.0.3 area 0
network 192.168.32.196 0.0.0.3 area 0
network 192.168.32.200 0.0.0.3 area 0

R4
hostname R4
int Se0/0/0
ip address 192.168.32.194 255.255.255.252
int Se0/0/1
ip address 192.168.32.177 255.255.255.252
int gig0/0
ip address 192.168.32.173 255.255.255.252
int gig0/1
ip address 192.168.32.181 255.255.255.252
router ospf 2
network 192.168.32.172 0.0.0.3 area 0
network 192.168.32.176 0.0.0.3 area 0
network 192.168.32.180 0.0.0.3 area 0
network 192.168.32.192 0.0.0.3 area 0

R5
hostname R5
int Se0/0/1
ip address 192.168.32.178 255.255.255.252
int se0/0/0
ip address 192.168.32.198 255.255.255.252
int gig0/0
ip address 192.168.32.185 255.255.255.252
int gig0/1
ip address 192.168.32.189 255.255.255.252
router ospf 2
network 192.168.32.176 0.0.0.3 area 0
network 192.168.32.184 0.0.0.3 area 0
network 192.168.32.188 0.0.0.3 area 0
network 192.168.32.188 0.0.0.3 area 0
network 192.168.32.196 0.0.0.3 area 0

MSW9
ip routing
hostname MSW9
int gig0/1
no switchport
ip address 192.168.32.174 255.255.255.252
int range fa0/1-3
channel-group 1 mode active
int port-channel 1
no switchport
ip address 192.168.32.161 255.255.255.252
vlan 6
name Soporte
vlan 7
name Seguridad
vlan 8
name DNS_Server
int gig0/2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 6,7,8
int vlan 6
ip address 192.168.32.34 255.255.255.224
standby 1 ip 192.168.32.33
standby 1 priority 150
standby 1 preempt
ip helper-address 192.168.12.182
int vlan7
ip address 192.168.32.66 255.255.255.224
standby 2 ip 192.168.32.65
standby 2 priority 150
standby 2 preempt
ip helper-address 192.168.12.182
int vlan8
ip address 192.168.32.2 255.255.255.224
standby 3 ip 192.168.32.1
standby 3 priority 150
standby 3 preempt
ip helper-address 192.168.12.182
router ospf 2
network 192.168.32.0 0.0.0.31 area 0
network 192.168.32.32 0.0.0.31 area 0
network 192.168.32.64 0.0.0.31 area 0
network 192.168.32.160 0.0.0.3 area 0
network 192.168.32.172 0.0.0.3 area 0

MSW10
ip routing
hostname MSW10
int gig0/1
no switchport
ip address 192.168.32.182 255.255.255.252
int range fa0/1-3
channel-group 1 mode active
int port-channel 1
no switchport
ip address 192.168.32.162 255.255.255.252
int range fa0/4-6
channel-group 2 mode active
int port-channel 2
no switchport
ip address 192.168.32.165 255.255.255.252
hostname S6
vlan 6
name Soporte
vlan 7
name Seguridad
vlan 8
name DNS_Server
int vlan 6
ip address 192.168.32.35 255.255.255.224
standby 1 ip 192.168.32.33
ip helper-address 192.168.12.182
int vlan 7
ip address 192.168.32.67 255.255.255.224
standby 2 ip 192.168.32.65
ip helper-address 192.168.12.182
int vlan 8
ip address 192.168.32.3 255.255.255.224
standby 3 ip 192.168.32.1
ip helper-address 192.168.12.182
router ospf 2
network 192.168.32.0 0.0.0.31 area 0
network 192.168.32.32 0.0.0.31 area 0
network 192.168.32.64 0.0.0.31 area 0
network 192.168.32.160 0.0.0.3 area 0
network 192.168.32.164 0.0.0.3 area 0
network 192.168.32.180 0.0.0.3 area 0


MSW11
ip routing
hostname MSW11
int range fa0/1-3
channel-group 3 mode active
int range fa0/4-6
channel-group 2 mode active
int port-channel 2
no switchport
ip address 192.168.32.166 255.255.255.252
int port-channel 3
no switchport
ip address 192.168.32.169 255.255.255.252
int gig0/2
no switchport
ip address 192.168.32.186 255.255.255.252
vlan 6
name Soporte
vlan 7
name Seguridad
interface gig0/1
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 6,7
int vlan 6
ip address 192.168.32.99 255.255.255.224
standby 3 ip 192.168.32.97
ip helper-address 192.168.12.182
int vlan 7
ip address 192.168.32.131 255.255.255.224
standby 4 ip 192.168.32.129
ip helper-address 192.168.12.182
router ospf 2
network 192.168.32.96 0.0.0.31 area 0
network 192.168.32.128 0.0.0.31 area 0
network 192.168.32.164 0.0.0.3 area 0
network 192.168.32.168 0.0.0.3 area 0
network 192.168.32.184 0.0.0.3 area 0

MSW12
ip routing
hostname MSW12
int range fa0/1-3
channel-group 3 mode active
int port-channel 3
no switchport
ip address 192.168.32.170 255.255.255.252
int gig0/2
no switchport
ip address 192.168.32.190 255.255.255.252
vlan 6
name Soporte
vlan 7
name Seguridad
int gig0/1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 6,7
int vlan 6
ip address 192.168.32.98 255.255.255.224
standby 3 ip 192.168.32.97
standby 3 priority 150
standby 3 preempt
ip helper-address 192.168.12.182
int vlan7
ip address 192.168.32.130 255.255.255.224
standby 4 ip 192.168.32.129
standby 4 priority 150
standby 4 preempt
ip helper-address 192.168.12.182
router ospf 2
network 192.168.32.96 0.0.0.31 area 0
network 192.168.32.128 0.0.0.31 area 0
network 192.168.32.168 0.0.0.3 area 0
network 192.168.32.188 0.0.0.3 area 0

S6
hostname S6
vlan 6
name Soporte
vlan 7
name Seguridad
vlan 8
name DNS_Server
int range gig0/1-2
switchport mode trunk
switchport trunk allowed vlan 6,7,8
int fa0/1
switchport mode trunk
switchport trunk allowed vlan 6,7
int fa0/11
switchport mode access
switchport access vlan 8
int fa0/12
switchport mode access
switchport access vlan 6
int fa0/13
switchport mode access
switchport access vlan 7

S7
hostname S7
vlan 6
name Soporte
vlan 7
name Seguridad
switchport trunk allowed vlan 6,7
int fa0/1
switchport mode trunk
switchport trunk allowed vlan 6,7
int range fa0/11-12
switchport mode access
switchport access vlan 6
int fa0/13
switchport mode access
switchport access vlan 7

```
