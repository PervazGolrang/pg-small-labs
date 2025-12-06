# Step 02 - Switch01

---

## 1. Basic configuration:

```bash
hostname SW01
!
spanning-tree mode rapid-pvst
!
lldp run
no cdp run
```

---

## 2.0 VLAN configuration:
```bash
!
vlan 101
 name CLIENT_SW01
vlan 102
 name CLIENT_SW02
vlan 103
 name CLIENT_SW03
vlan 170
 name PRINTER
vlan 199
 name Management
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan101
 ip address 10.47.39.2 255.255.255.0
!
interface Vlan102
 ip address 10.47.40.2 255.255.255.0
!
interface Vlan103
 ip address 10.47.41.2 255.255.255.0
!
interface Vlan170
 ip address 10.47.170.2 255.255.255.0
!
interface Vlan199
 ip address 10.47.199.2 255.255.255.0
!
interface range vlan101-103, vlan170, vlan199
 no shutdown
!
```

Do note that I am utilizing SVI, if I wanted the traffic to be seen by the firewall, then SVI has to be removed, else, it will not pass to the firewall for logging.

---

## 3.0 Interface configuration:
```bash
interface GigabitEthernet0/0
 description UPLINK to FW01
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/1
 description DOWNLINK to SW02
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 description DOWNLINK to SW03
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/3
 description A: CLIENT
 switchport access vlan 101
 switchport mode access
 switchport protected
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
```
