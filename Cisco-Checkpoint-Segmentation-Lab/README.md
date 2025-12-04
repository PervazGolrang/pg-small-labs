# Cisco & Check Point Micro-Segmentation Lab

This is a small lab to implement **Zero Trust Access Layer** design by mitigating east-west lateral movement (e.g. ransomware) by combining legacy **Cisco Protected Ports** (Layer 2 isolation) with **Checkpoint Firewall Policies** (Layer 3 segmentation).

Note:
The **Legacy** command `switchport protected` blocks unicast, multicast, and broadcast to all interfaces that have the `protected` configuration. This is a strong legacy feature, supporting even Catalyst 2960 (non-X and non-S). This is a cheap and easy alternative compared to PVLANS. The downside is reduced scalability and functionality, e.g. PVLANs `community`.

The design enforced a "VLAN per Switch" architecture, ensuring that all inter-switch traffic must traverse the Firewall for inspection, while intra-switch traffic is blocked at the ASIC level.

---

## Platform
| Device     | Image               |
|------------|---------------------|
| Switch     | vIOS - Version 15.2 |
| Firewall   | GAiA R82            |
| PC/Printer | Alpine Linux 3.21   |

Implemented in Cisco Modeling Labs  (CML)

---

## Topology
![cisco_checkpoint_micro-segment-lab.png](/Cisco-Checkpoint-Segmentation-Lab/Images/cisco_checkpoint_micro-segment-lab.png.png)

## Design Logic
1. **Aggregator (SW01):** Trunks all VLANs to the Firewall.
2. **Access (SW02/03):** Hosts specific Client VLANs.
3. **Firewall:** Routes between VLANs and applies security policy.

---

## Core Features

| Component           | Description                                                                                                       |
|---------------------|-------------------------------------------------------------------------------------------------------------------|
| **L2 Isolation**    | `switchport protected` prevents peers on the same switch from communicating.                                      |
| **L3 Segmentation** | Unique VLANs per switch force traffic upstream to the Firewall.                                                   |
| **Security Policy** | Check Point Access Control Policy allows business traffic (Printing/Internet) but drops East-West client traffic. |
| **DHCP Services**   | Firewall acts as DHCP server with Option 3 (Router) injection for correct routing.                                |