# Manual Técnico - Proyecto 1 REDES

GRUPO 13

- Raudy David Cabrera Contreras - 201901973
- Alejandro René Caballeros González - 201903549

## Objetivos

- **Demostrar el conocimiento adquirido** sobre la creación de VLANS y el protocolo VTP, lo que permitirá la segmentación lógica de la red para mejorar el rendimiento y la seguridad.
- **Demostrar el conocimiento adquirido** sobre el Spanning Tree Protocol, que garantiza la redundancia y previene los bucles en la red.
- **Desarrollar una topología de red** en Packet Tracer según las especificaciones dadas.
- **Capturar paquetes** utilizando el modo simulación de Packet Tracer.


# Manual Técnico de Implementación de Red

## 1. Resumen de Direcciones IP y VLAN

### Escuintla
| VLAN | Nombre | Red | Máscara | Hosts Disponibles | Gateway |
|------|---------|-----|---------|------------------|---------|
| 19 | RHH | 192.148.13.32/29 | 255.255.255.248 | 6 | 192.148.13.33 |
| 39 | Ventas | 192.148.13.0/27 | 255.255.255.224 | 30 | 192.148.13.1 |

**Justificación de máscaras:**
- VLAN 19 (RHH): Se utilizó /29 para optimizar el espacio, permitiendo hasta 6 hosts
- VLAN 39 (Ventas): Se asignó /27 para permitir mayor crecimiento con hasta 30 hosts

### Izabal
| VLAN | Nombre | Red | Máscara | Hosts Disponibles | Gateway |
|------|---------|-----|---------|------------------|---------|
| 19 | RRHH | 192.167.13.32/28 | 255.255.255.240 | 14 | 192.167.13.33 |
| 29 | Contabilidad | 192.167.13.48/29 | 255.255.255.248 | 6 | 192.167.13.49 |
| 39 | Ventas | 192.167.13.0/27 | 255.255.255.224 | 30 | 192.167.13.1 |

**Justificación de máscaras:**
- VLAN 19 (RRHH): Máscara /28 permite hasta 14 hosts
- VLAN 29 (Contabilidad): /29 optimiza el espacio para 6 hosts
- VLAN 39 (Ventas): /27 permite crecimiento con 30 hosts disponibles

### Petén
| VLAN | Nombre | Red | Máscara | Hosts Disponibles | Gateway |
|------|---------|-----|---------|------------------|---------|
| 19 | RRHH | 192.158.13.48/28 | 255.255.255.240 | 14 | 192.158.13.49 |
| 39 | Ventas | 192.158.13.0/27 | 255.255.255.224 | 30 | 192.158.13.1 |
| 49 | Informática | 192.158.13.32/28 | 255.255.255.240 | 14 | 192.158.13.33 |

**Justificación de máscaras:**
- VLAN 19 (RRHH): /28 permite hasta 14 hosts
- VLAN 39 (Ventas): /27 para mayor escalabilidad con 30 hosts
- VLAN 49 (Informática): /28 optimiza espacio para 14 hosts

### Quiché
| VLAN | Nombre | Red | Máscara | Hosts Disponibles | Gateway |
|------|---------|-----|---------|------------------|---------|
| 19 | RRHH | 192.178.13.96/28 | 255.255.255.240 | 14 | 192.178.13.97 |
| 29 | Contabilidad | 192.178.13.112/28 | 255.255.255.240 | 14 | 192.178.13.113 |
| 39 | Ventas | 192.178.13.0/26 | 255.255.255.192 | 62 | 192.178.13.1 |
| 49 | Informática | 192.178.13.64/27 | 255.255.255.224 | 30 | 192.178.13.65 |

**Justificación de máscaras:**
- VLAN 19 y 29: /28 permite hasta 14 hosts cada una
- VLAN 39: /26 permite mayor crecimiento con 62 hosts
- VLAN 49: /27 optimiza para 30 hosts

### Jutiapa
| VLAN | Nombre | Red | Máscara | Hosts Disponibles | Gateway |
|------|---------|-----|---------|------------------|---------|
| 19 | RRHH | 192.168.13.48/28 | 255.255.255.240 | 14 | 192.168.13.49 |
| 29 | Contabilidad | 192.168.13.64/29 | 255.255.255.248 | 6 | 192.168.13.65 |
| 39 | Ventas | 192.168.13.0/27 | 255.255.255.224 | 30 | 192.168.13.1 |
| 49 | Informática | 192.168.13.32/28 | 255.255.255.240 | 14 | 192.168.13.33 |

**Justificación de máscaras:**
- VLAN 19 y 49: /28 permite hasta 14 hosts cada una
- VLAN 29: /29 optimiza para 6 hosts
- VLAN 39: /27 permite crecimiento con 30 hosts

### Interconexión entre Routers
Se utilizó la red 10.0.0.0/30 para las conexiones punto a punto entre routers, subdividida en múltiples subredes /30 para cada enlace. Esta máscara es óptima para enlaces punto a punto ya que solo requieren 2 direcciones IP utilizables.

## 2. Configuración por Dispositivo

### Configuración de Switches

#### Configuración Básica (Común para todos los switches)
```cisco
enable
conf t
spanning-tree mode rapid-pvst
```

#### Configuración VTP para Switches Servidor
```cisco
vtp mode server
vtp domain [NOMBRE_DOMINIO]
vtp password [PASSWORD]

! Creación de VLANs
vlan [ID]
name [NOMBRE]
exit
```

#### Configuración VTP para Switches Cliente
```cisco
vtp mode client
vtp domain [NOMBRE_DOMINIO]
vtp password [PASSWORD]
```

#### Configuración de Interfaces
```cisco
! Interfaces Trunk
interface fastEthernet0/1
switchport mode trunk
no shutdown

! Interfaces de Acceso
interface fastEthernet0/11
switchport mode access
switchport access vlan [ID]
no shutdown
```

### Configuración de Routers

#### Configuración Básica de Interfaces
```cisco
enable
conf t
interface fastEthernet5/0.[VLAN-ID]
encapsulation dot1q [VLAN-ID]
ip address [IP] [MASCARA]
no shutdown
```

#### Configuración de ACLs
```cisco
access-list 100 deny ip [RED_ORIGEN] [WILDCARD] [RED_DESTINO] [WILDCARD]
access-list 100 permit ip any any

interface fastEthernet5/0.[VLAN-ID]
ip access-group 100 in
```

#### Configuración de Protocolos de Enrutamiento

##### EIGRP
```cisco
router eigrp 1
network [RED] [WILDCARD]
redistribute ospf 1
redistribute rip
no auto-summary
```

##### OSPF
```cisco
router ospf 1
network [RED] [WILDCARD] area 0
redistribute eigrp 1
redistribute rip
```

##### RIP
```cisco
router rip
version 2
network [RED]
redistribute eigrp 1
redistribute ospf 1
no auto-summary
```

### Configuración de PCs
```cisco
! Configuración IP estática
ip address [IP] [MASCARA]
gateway [GATEWAY]
```

## 3. Verificación y Mantenimiento

### Comandos de Verificación

#### Switches
```cisco
show vlan brief
show spanning-tree
show vtp status
show interfaces trunk
```

#### Routers
```cisco
show ip route
show ip interface brief
show access-lists
show ip protocols
```

### Comandos de Mantenimiento
```cisco
write memory
copy running-config startup-config
show running-config
show startup-config
```

## Topologia

![image](img/31.png)
