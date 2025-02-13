# PRACTICA 1 - REDES DE COMPUTADORAS 2
## Integrantes
| Nombre   | Carnet    |
|----------|----------|
| Diego Andres Huite Alvarez  | 202003585  |
| Gerhard Benjamin Ardon Valdez  | 202004796  |
| Pablo Javier Batz Contreras  | 201902698  |
| José Manuel Lacán Chavajay   | 201900087  |

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
