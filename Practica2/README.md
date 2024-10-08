# Manual Técnico - Práctica 1 REDES

## Objetivos
- **Demostrar conocimiento adquirido** respecto a la agregación de enlaces.
- **Demostrar conocimiento adquirido** para la creación de rutas estáticas.
- **Demostrar conocimiento adquirido** respecto a la puerta de enlace predeterminada, así como también para el manejo de protocolos de redundancia en la misma.

---

## Descripción del Proyecto

Después de realizar un buen trabajo para la empresa “Solución al Cliente S.A.”, el camino de la vida lo guía hacia una nueva contratación, esta vez por la administración del Colegio CiscoWorks, quienes desean expandir su oferta educativa y como resultado fue creada la Academia Técnica de Formación Empresarial – ACATEC.

Recientemente ellos se interesaron en la infraestructura para la comunicación entre ambos sitios, por lo que se le encargó el realizar una simulación de cómo sería la topología de este, incluyendo acceso a las redes privadas y redundancia de enlaces.

---

## Configuración de routers R1, R2 y R6

### Router R1

R1 actúa como el router principal que conecta las dos redes (132.168.0.0/24 y 132.178.0.0/24).

```
enable
configure terminal
interface fa0/0
ip address 132.168.1.2 255.255.255.248
no shutdown
exit
interface fa0/1
ip address 132.168.2.2 255.255.255.248
no shutdown
exit
interface se0/0
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
ip route 132.168.0.0 255.255.255.0 132.168.1.1
ip route 132.178.0.0 255.255.255.0 10.0.0.2
```

### Router R2

R2 es parte del grupo HSRP en la red 132.168.0.0/24.

```
enable
configure terminal
interface fa0/0
ip address 132.168.1.1 255.255.255.248
no shutdown
exit
interface fa0/1
ip address 132.168.0.2 255.255.255.0
no shutdown
standby 1 ip 132.168.0.1
standby 1 priority 110
standby 1 preempt
exit
ip route 0.0.0.0 0.0.0.0 132.168.1.2
```

### Router R6

R6 es parte del grupo HSRP en la red 132.178.0.0/24.

```
enable
configure terminal
interface fa0/0
ip address 132.178.2.2 255.255.255.248
no shutdown
exit
interface fa0/1
ip address 132.178.0.3 255.255.255.0
no shutdown
standby 1 ip 132.178.0.1
exit
ip route 0.0.0.0 0.0.0.0 132.178.2.1
```

---

## Configuración de switch SW1 y SW3

### Switch SW1

SW1 está configurado con PortChannel usando PAGP.

```
enable
configure terminal
interface range fa0/3 - 4
channel-group 1 mode desirable
exit
interface port-channel 1
switchport mode trunk
exit
```

### Switch SW3

SW3 está configurado con PortChannel usando LACP.

```
enable
configure terminal
interface range fa0/1 - 2
channel-group 1 mode active
exit
interface port-channel 1
switchport mode trunk
exit
```

---

## Configuración de VPC11 y VPC14

### VPC11
```
ip 132.168.0.4 255.255.255.0 132.168.0.1
```
### VPC11
```
ip 132.178.0.5 255.255.255.0 132.178.0.1
```

---

## Comandos Usados

### Creación de ruta estática

Para crear una ruta estática, se utiliza el comando ip route seguido de la red de destino, la máscara de subred y la dirección IP del siguiente salto o la interfaz de salida.

```
ip route 0.0.0.0 0.0.0.0 132.168.1.2
```

### Creación de PortChannel con PAGP y LACP 

#### PAGP (usado en SW0-SW1)

```
interface range fa0/1 - 2
channel-group 1 mode desirable
```

#### LACP (usado en SW2-SW3)

```
interface range fa0/3 - 4
channel-group 1 mode active
```

### Creación de IP virtual con HSRP

```
interface fa0/1
standby 1 ip 132.168.0.1
standby 1 priority 110
standby 1 preempt
```

### Configuración de VPC

```
ip [dirección IP] [máscara de subred] [gateway]
```

---

## Comandos para la verificación

### Verificar Estado de Interfaces

```
show ip interface brief
```

Este comando muestra un resumen de todas las interfaces, su estado y dirección IP.

### Verificar Rutas Configuradas

```
show ip route
```

Muestra la tabla de enrutamiento del router, incluyendo rutas estáticas y dinámicas.

### Verificar Configuración HSRP

```
show standby brief
```

Proporciona un resumen de la configuración HSRP en el router


### Verificar Configuración PortChannel

```
show etherchannel summary
```

Muestra un resumen de los grupos EtherChannel configurados en el switch.


### Prueba de Conectividad

En las VPCs, se puede usar el comando ping para verificar la conectividad:

```
ping 132.178.0.5
```
Este comando envía paquetes ICMP a la dirección especificada para probar la conectividad.






