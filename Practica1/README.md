# PRACTICA 1 - REDES DE COMPUTADORAS 2
## Integrantes
| Nombre   | Carnet    |
|----------|----------|
| Diego Andres Huite Alvarez  | 202003585  |
| Gerhard Benjamin Ardon Valdez  | 202004796  |
| Pablo Javier Batz Contreras  | 201902698  |
| José Manuel Lacán Chavajay   | 201900087  |

#Configuración basica Switch
```
enable
configure terminal
no ip domain-lookup
hostname SW0_G2
enable secret redes2grupo2 (Solo en Switch0 y Switch11)


#Configuración modo troncal 

```
enable
configure terminal
interface range Fa0/1-n
switchport mode trunk
switchport trunk allowed vlan all
do wr

```

#Configuración VTP, Configuración Servidor
## Para Switch0 y Switch11

```
enable
configure terminal
vtp mode server
vtp domain g2
vtp password redes2grupo2
vtp version 2
do wr
show vtp status

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
# Configuración VLAN solo en Switches Servidores:  Switch0 y Switch11

## Edificio: Izquierdo
```

enable
configure terminal
vlan 12
name Primaria
vlan 22
name Basicos
vlan 32
name Diversificado

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
# Configuracion Acces moede Para las interfaces conectadas a las PC

```
enable
configure terminal
interface fa0/11
switchport mode access
switchport Access vlan 12 (para primaria)
do wr
show interface status
```

```
```
---

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

---
