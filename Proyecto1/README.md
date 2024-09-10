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

### Configurar switch como servidor (SW1)

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








