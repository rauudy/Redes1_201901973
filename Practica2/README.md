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

## Topologia Final

![image](https://github.com/user-attachments/assets/ce145041-60fd-4cff-99ae-a2440d1bc902)

![image](https://github.com/user-attachments/assets/3f19b923-22b8-42c1-a8f4-7b5e42db0f46)

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

![image](https://github.com/user-attachments/assets/c5f9ac4d-b33e-4715-af53-f9d2e90fa289)

![image](https://github.com/user-attachments/assets/f87fa38b-dc5d-4a45-8eb5-e7f803da9299)

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

![image](https://github.com/user-attachments/assets/e3a458fa-9d1f-40a2-9830-57e35525295c)

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

![image](https://github.com/user-attachments/assets/ef6224c4-8236-4924-9c11-ce69ea0f3a19)

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

![image](https://github.com/user-attachments/assets/71b70c40-f6c4-47d7-91dd-2ddf1609196a)

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

![image](https://github.com/user-attachments/assets/58e7d2ea-2456-46d8-8282-ae0ac691707e)

---

## Configuración de VPC11 y VPC14

### VPC11
```
ip 132.168.0.4 255.255.255.0 132.168.0.1
```

![image](https://github.com/user-attachments/assets/f012b6b0-69f6-4e02-8f24-dbc0c8fe0725)

### VPC14
```
ip 132.178.0.5 255.255.255.0 132.178.0.1
```

![image](https://github.com/user-attachments/assets/1c0e90ec-6956-46ef-86c6-52a822024bdc)

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
![image](https://github.com/user-attachments/assets/b6068134-1dce-4c73-8676-e78b03e72c43)

#### LACP (usado en SW2-SW3)

```
interface range fa0/3 - 4
channel-group 1 mode active
```
![image](https://github.com/user-attachments/assets/d7bdd4d4-7265-4d97-96d2-9322ba088777)

### Creación de IP virtual con HSRP

```
interface fa0/1
standby 1 ip 132.168.0.1
standby 1 priority 110
standby 1 preempt
```
![image](https://github.com/user-attachments/assets/bca8654a-ec1b-4d1e-b828-cc70e46f4267)

### Configuración de VPC

```
ip [dirección IP] [máscara de subred] [gateway]
```
![image](https://github.com/user-attachments/assets/781106ae-7c4c-45d9-9fc3-ae78370b101b)

---

## Comandos para la verificación

### Verificar Estado de Interfaces

```
show ip interface brief
```
R1:
![image](https://github.com/user-attachments/assets/d1b59e38-18c1-4048-a3ed-add8814e36a4)

R2:
![image](https://github.com/user-attachments/assets/a2de71f1-eed2-45d7-bab3-40ec27d73fea)

R6:
![image](https://github.com/user-attachments/assets/bc5ccd66-0599-41a4-936f-44416cce3433)


Este comando muestra un resumen de todas las interfaces, su estado y dirección IP.

### Verificar Rutas Configuradas

```
show ip route
```

R1:
![image](https://github.com/user-attachments/assets/e9949b96-ada9-4789-a67b-4a4fd8cbc2c2)


Muestra la tabla de enrutamiento del router, incluyendo rutas estáticas y dinámicas.

### Verificar Configuración HSRP

```
show standby brief
```

R2:
![image](https://github.com/user-attachments/assets/a4818c33-93f3-48ca-a04d-45f0dadc7375)


Proporciona un resumen de la configuración HSRP en el router


### Verificar Configuración PortChannel

```
show etherchannel summary
```
![image](https://github.com/user-attachments/assets/21041434-95e9-4c75-9abf-f7f2ad89a72e)

Muestra un resumen de los grupos EtherChannel configurados en el switch.


### Prueba de Conectividad

En las VPCs, se puede usar el comando ping para verificar la conectividad:

```
ping 132.178.0.5
```
Este comando envía paquetes ICMP a la dirección especificada para probar la conectividad.

VPC11 - VPC13:
![image](https://github.com/user-attachments/assets/9302ce2c-4934-47b2-bea9-3b93cda0bd47)

VPC12 - VCP11:
![image](https://github.com/user-attachments/assets/4caff57c-2077-4f26-b2f6-d10445c5207f)






