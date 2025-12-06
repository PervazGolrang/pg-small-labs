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

Note: This configuration does use SVIs on the switch, so inter‑VLAN and routed traffic is terminated on SW01. If I wanted traffic to be logged by the firewall, the firewall must be the routing gateway for those VLANs. Alternatively, I can use a different method by using a SPAN session to send a copy of the traffic to the firewall for monitoring or logging, however, that would be unecessarily complicated than just removing SVI in general.

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
