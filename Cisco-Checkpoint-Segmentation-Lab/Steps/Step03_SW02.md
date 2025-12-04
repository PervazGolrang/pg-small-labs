# Step 03 - Switch02

---

## 1. Basic configuration:

```bash
hostname SW02
!
spanning-tree mode rapid-pvst
!
lldp run
no cdp run
```

---

## 2.0 VLAN configuration:
```bash
vlan 102
 name CLIENT_SW02
vlan 170
 name PRINTER
!
interface Vlan1
 no ip address
 shutdown
!
```

---

## 3.0 Interface configuration:
```bash
interface GigabitEthernet0/0
 description UPLINK to SW01
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/1
 description A: CLIENT
 switchport access vlan 102
 switchport mode access
 switchport protected
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
interface GigabitEthernet0/2
 description A: CLIENT
 switchport access vlan 170
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
```