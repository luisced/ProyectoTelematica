# ProyectoTelematica


## Global Config

en
conf t
enable secret cisco

## VLAN

enable
conf t
hostname f"sw{numero_switch}"
ip domain-name cisco.com
int vlan 1
ip address 192.168.0.{vlan} 255.255.255.0

## Gateway

ip default-gateway 192.168.0.1
no shutdown
do write
exit

2

## SSH

enable
configure terminal
crypto key generate rsa
4096
ip ssh version 2
username cisco secret cisco
line vty 0 15
logging synchronous
transport input ssh
login local
exit

## Configure VLAN

enable
configure terminal

### 2. Create VLAN 10 and VLAN 20:

vlan 10
name VLAN10
exit
vlan 20
name VLAN20
exit

## **Assigning Ports to VLANs:**

### 1. Assign interfaces to VLAN 10:

interface range fa0/1 - 10
switchport mode access
switchport access vlan 10
exit

### 2. Assign interfaces to VLAN 20:

interface range fa0/11 - 20
switchport mode access
switchport access vlan 20
exit

#### Please replace `fa0/1 - 10` and `fa0/11 - 20` with the actual interface range on your switch.

## **Save the Configuration:**

enable
show running-config

```
Router# write memory
   -- or --
Router# copy running-config startup-config 
```

copy running-config flash:Mau

#### When you use the `copy running-config startup-config` command, you may be prompted to confirm the destination filename. In most cases, just press Enter to accept the default.

show dir flash

# Routing Configuration

en
conf t
hostname r1
ip domain-name cisco.com

## Subneting

enable
configure terminal
interface GigabitEthernet0/1
ip address 172.16.10.14 255.255.255.252
no shutdown
exit

## Mostrar ip

show ip interface brief

## **Verificar conectividad:**

ping 172.16.12.14

## 3. **Verificar las rutas:**

show ip route

# Examen

## Parte 2

### Dist-SW1

enable
configure terminal
hostname DIST-SW1
ip domain-name up.edu.mx
interface vlan 10
ip address 192.168.10.20
no shutdown
exit
ip default-gateway 192.168.10.1
username parcial1 secret cisco123
enable secret cisco
crypto key generate rsa
yes
1024
ip ssh version 2
line vty 0 15
transport input ssh
login local
exec-timeout 0 0
logging synchronous
exit
line console 0
login local
exec-timeout 0 0
logging synchronous
exit
exit
write memory
copy running-config startup-config DIST-SW1

### Dist-SW2

enable
configure terminal
hostname DIST-SW2
ip domain-name up.edu.mx
interface vlan 10
ip address 192.168.10.21
no shutdown
exit
ip default-gateway 192.168.10.1
username parcial1 secret cisco123
enable secret cisco
crypto key generate rsa
yes
1024
ip ssh version 2
line vty 0 15
transport input ssh
login local
exec-timeout 0 0
logging synchronous
exit
line console 0
login local
exec-timeout 0 0
logging synchronous
exit
exit
write memory
copy running-config startup-config DIST-SW1

### ACC-SW1

enable
configure terminal
hostname ACC-SW1
ip domain-name up.edu.mx
interface vlan 10
ip address 192.168.10.22
no shutdown
exit
ip default-gateway 192.168.10.1
username parcial1 secret cisco123
enable secret cisco
crypto key generate rsa
yes
1024
ip ssh version 2
line vty 0 15
transport input ssh
login local
exec-timeout 0 0
logging synchronous
exit
line console 0
login local
exec-timeout 0 0
logging synchronous
exit
exit
write memory
copy running-config startup-config ACC-SW1

### ACC-SW2

enable
configure terminal
hostname ACC-SW2
ip domain-name up.edu.mx
interface vlan 10
ip address 192.168.10.23
no shutdown
exit
ip default-gateway 192.168.10.1
username parcial1 secret cisco123
enable secret cisco
crypto key generate rsa
yes
1024
ip ssh version 2
line vty 0 15
transport input ssh
login local
exec-timeout 0 0
logging synchronous
exit
line console 0
login local
exec-timeout 0 0
logging synchronous
exit
exit
write memory
copy running-config startup-config ACC-SW2

## Parte 3

# En cada switch (ACC-SW1, ACC-SW2, DIST-SW1, DIST-SW2):

enable
configure terminal
vlan 10
name IT
exit
vlan 20
name Students
exit
vlan 30
name Teachers
exit

# En los Switches de Acceso (ACC-SW1, ACC-SW2):

interface Vlan10
 ip address 192.168.10.22 255.255.255.0
 no shutdown

interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10

interface GigabitEthernet0/1
switchport mode dynamic auto
switchport trunk native vlan 10
exit

# En los Switches de Distribución (DIST-SW1, DIST-SW2):

interface GigabitEthernet 1/0/1
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30
exit

interface GigabitEthernet 1/0/2
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30
exit

interface GigabitEthernet 1/0/3
switchport mode trunk
switchport trunk native vlan 10
switchport trunk allowed vlan 10,20,30
exit

show vlan brief
show running-config | include interface|switchport
show interfaces trunk

# Asumiendo que fa0/1 es para PC 1 IT

interface FastEthernet 0/1
switchport mode access
switchport access vlan 10
exit

interface FastEthernet 0/2
switchport mode access
switchport access vlan 20
exit

interface FastEthernet 0/3
switchport mode access
switchport access vlan 30
exit

# Parte 4

## Dist 1

spanning-tree mode rapid-pvst
spanning-tree vlan 10 priority 4096
spanning-tree vlan 20 priority 8192
spanning-tree vlan 30 priority 4096

### Dist 2
spanning-tree mode rapid-pvst
spanning-tree vlan 10 priority 8192
spanning-tree vlan 20 priority 4096
spanning-tree vlan 30 priority 4096

## ACC-SW1 y ACC-SW2:

spanning-tree mode rapid-pvst

interface GigabitEthernet 1/0/2
spanning-tree cost 3
end

interface GigabitEthernet 1/0/1
spanning-tree cost 3
end

interface GigabitEthernet 1/1/3
spanning-tree cost 3
end

## BPDUGuard

enable
configure terminal
spanning-tree portfast bpduguard default
interface [tipo_de_interfaz] [número_de_interfaz]
spanning-tree bpduguard enable
exit

spanning-tree vlan 30 root primary
spanning-tree vlan 30 root secondary
