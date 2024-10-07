# Manual Técnico - Práctica 1 REDES

## Objetivos
- **Demostrar conocimiento adquirido** en los protocolos Ethernet, IP, ARP e ICMP.
- **Configurar máquinas virtuales VPC** y switches de capa 2.
- **Desarrollar una topología de red** en Packet Tracer según las especificaciones dadas.
- **Capturar paquetes** utilizando el modo simulación de Packet Tracer.

---

## Descripción del Proyecto

Una empresa ofrece una plaza como técnico de redes, y como parte del proceso de evaluación, se solicita al aspirante demostrar su conocimiento utilizando la herramienta Packet Tracer.

El aspirante debe diseñar la topología de una red para un negocio de tres niveles, utilizando una **topología en estrella** y teniendo en cuenta el cableado estructurado. La red propuesta conectará tres switches entre los niveles, con cada switch encargado de un nivel específico.

---

## Configuración de routers R1, R2 y R6

### R1

#### Configura la interfaz serial

```
enable
configure terminal
interface serial 0/0
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
```

### Configuracion de las interfaces FastEthernet:

```
enable
configure terminal
interface fastEthernet 0/0
ip address 132.168.1.2 255.255.255.248
no shutdown
exit

interface fastEthernet 0/1
ip address 132.168.2.2 255.255.255.248
no shutdown
exit
```

### Configuración de la Ruta Estática:

```
enable
configure terminal
ip route 132.178.0.0 255.255.255.0 10.0.0.2
```

### R4

#### Configura la interfaz serial

```
enable
configure terminal
interface serial 0/0
ip address 10.0.0.2 255.255.255.252
no shutdown
exit
```

### Configuracion de las interfaces FastEthernet:

```
enable
configure terminal
interface fastEthernet 0/0
ip address 132.178.1.1 255.255.255.248
no shutdown
exit

interface fastEthernet 0/1
ip address 132.178.2.1 255.255.255.248
no shutdown
exit
```

### Configuración de la Ruta Estática:

```
enable
configure terminal
ip route 132.168.0.0 255.255.255.0 10.0.0.1
```


### R6

### Configuracion de las interfaces FastEthernet:

```
enable
configure terminal
interface fastEthernet 0/0
ip address 132.178.2.2 255.255.255.248
no shutdown
exit
```

---

## Configuración de switch SW1 y SW3

### SW1

#### Configura PAGP

```
enable
configure terminal
interface range fastEthernet 0/3-4
channel-group 1 mode auto
exit
interface port-channel 1
switchport mode trunk
exit
```

### SW3

#### Configura LACP

```
enable
configure terminal
interface range fastEthernet 0/1-2
channel-group 1 mode passive
exit
interface port-channel 1
switchport mode trunk
exit
```

--- 

## Configuración de VPC11 y VPC14

