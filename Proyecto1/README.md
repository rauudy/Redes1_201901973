# Manual Técnico - Práctica 1 REDES

## Objetivos
- **Demostrar el conocimiento adquirido** sobre la creación de VLANS y el protocolo VTP, lo que permitirá la segmentación lógica de la red para mejorar el rendimiento y la seguridad.
- **Demostrar el conocimiento adquirido** sobre el Spanning Tree Protocol, que garantiza la redundancia y previene los bucles en la red.
- **Desarrollar una topología de red** en Packet Tracer según las especificaciones dadas.
- **Capturar paquetes** utilizando el modo simulación de Packet Tracer.

---

## Resumen de direcciones IP y VLAN

Terminaciones de carnet: 9 y 3. La suma 9+3 = 12 y se usara el ultimo digito **2**

| Departamento | VLAN ID | Red           |
|--------------|---------|---------------|
| Contabilidad | 22      | 192.168.22.0/24 |
| Secretaría   | 32      | 192.168.32.0/24 |
| RRHH         | 42      | 192.168.42.0/24 |
| IT           | 52      | 192.168.52.0/24 |

---


## 2. Capturas de la implementación de las topologías

### Centro Administrativo
![image](img/31.png)

### Backbone
![image](img/32.png)

### Área de trabajo
![image](img/33.png)

### Vista Completa
![image](img/30.png)

## 3. Detalle de los comandos usados

### Configuración de Switch como Servidor (SW1)

``` 
enable
configure terminal
vtp mode server
vtp domain P13 
vtp password usac
vtp version 2
exit
wr 
show vtp status  
```
![image](img/2.png)

### Configuración de los demás Switch como Cliente

``` 
enable
configure terminal
vtp mode client
vtp domain P13 
vtp password usac
vtp version 2
exit
wr
show vtp status
```
![image](img/3.png)


### Configuración de un Switch como Transparente (SW9)

``` 
enable
configure terminal
vtp mode transparent
vtp version 2
exit
wr
show vtp status
```
![image](img/4.png)
![image](img/5.png)


### Configuración de Modo Troncal entre Switches 2960

``` 
enable 
configure terminal
interface range f0/1-7
switchport mode trunk
switchport trunk allowed vlan all
exit 
exit 
wr
show run //para verificar si se configuró el modo troncal
```
![image](img/6.png)
![image](img/7.png)


### Configuración de Modo Troncal entre Switches 3560

``` 
enable 
configure terminal
interface range f0/1-7
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan all
exit 
exit 
wr
show run //para verificar si se configuró el modo troncal
```
![image](img/8.png)
![image](img/9.png)


### Configuración de las VLANS desde el servidor

``` 
enable
configure terminal
vlan 22
name Contabilidad
exit
vlan 32
name Secretaria
exit
vlan 42
name RRHH
exit
vlan 52
name IT
exit
exit
show vlan //mostrar los vlans
```
![image](img/10.png)
![image](img/11.png)


### Configuración de los Switch a todas las PC los VLANS en los f0/#

- **Área: Contabilidad**  
``` 
enable 
configure terminal
interface f0/13
switchport mode access
switchport access vlan 22
no shutdown
exit
exit
wr
exit
```

- **Área: Secretaria**  
``` 
enable 
configure terminal
interface f0/11
switchport mode access
switchport access vlan 32
no shutdown
exit
exit
wr
exit
```

- **Área: RRHH**  
``` 
enable 
configure terminal
interface range f0/12-13
switchport mode access
switchport access vlan 42
no shutdown
exit
exit
wr
exit
```

- **Área: IT**  
``` 
enable 
configure terminal
interface f0/12
switchport mode access
switchport access vlan 52
no shutdown
exit
exit
wr
exit
```
``` 
interface gi1/0/3
interface range gi1/0/3-4
```

![image](img/12.png)
![image](img/14.png)
![image](img/15.png)


### Configuración de RSTP PVST al ROOT (SW1)

``` 
enable
configure terminal
spanning-tree mode rapid-pvst
spanning-tree vlan 1 root primary
exit
wr
show spanning-tree
```
![image](img/20.png)


### Configuración de RSTP PVST para los demás Switch

``` 
enable
configure terminal
spanning-tree mode rapid-pvst
exit
wr
show spanning-tree
```
![image](img/21.png)
![image](img/22.png)
![image](img/23.png)
![image](img/24.png)
![image](img/25.png)


## 4. Ping entre hosts

- **IP PC: IT**  
![image](img/16.png)

- **IP PC: RRHH**  
![image](img/17.png)

- **IP PC: CONTABILIDAD**  
![image](img/18.png)

- **IP PC: SECRETARIA**  
![image](img/19.png)

- Validar rstp (-t ping extendido)
``` 
ping -t 192.168.xx.xx
```

#### Área de RRHH

- **Ping dentro del área de RRHH**

![image](img/26.png)

- **Ping fuera del área de RRHH**

![image](img/27.png)

#### Área de Contabilidad

- **Ping dentro del área de Contabilidad**

![image](img/28.png)

- **Ping fuera del área de Contabilidad**

![image](img/29.png)


## Demostración de Funcionanmiento RSTP

<https://youtu.be/1B0nsWt_zoQ>

