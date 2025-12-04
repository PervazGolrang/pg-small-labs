# Step 01 - Implementation & Configuration

This step is configuration on the Firewall, including GAiA and Smartconsole.

**Note:**  
Step 1 - 4 are in GAiA portal  
Step 5 - 6 are in SmartConsole  

---

## 1. Setting up the Firewall Wizard

First setup is in the CLI, so then I can use GAiA and Smartconsole.

### Interface Setup (Gaia CLI)
```bash
# External Interface (NAT)
set interface eth1 ipv4-address 192.168.40.200 mask-length 24
set static-route default nexthop gateway address 192.168.40.1 on
set primary dns 192.168.40.1
```
![Step01 - Smartconsole Login](/Cisco-Checkpoint-Segmentation-Lab/Images/step01_smartconsole_login.png)  

[Step01 - SMS-Wizard](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_SMS-wizard.png)  

[Step01 - Smartconsole Session](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_Smartconsole_session.png)  

The IPs shown int he images are incorrect, as it is a copy from a previous lab, logic follows the same. 

---

## 2. Setting up VLANs

Correct VLAN set on eth1 will be implemented, derived from [IP_Plan.md](/Cisco-Checkpoint-Segmentation-Lab/docs/IP_Plan.md).

![Step01 - VLAN creation](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_VLANs.png)

---

## 3. Setting up DHCP

DHCP Server has to be enabled at the top `Enable DHCP Server`, and in the configuration-box `Enable DHCP Subnet`.

The network address and subnet mask can be grabbed from the **Network Interfaces** from selecting `Get from interface..`.

![Step01 - DHCP creation](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_DHCP.png)

Remember to include **address pool**, as by default, it will **NOT** select any. This will cause that the Alpine nodes will not recieve an IP Address.

---

## 4. Setting up LLDP

LLDP was a choice to set up. By default when enabling, all TLVs are non-selected. This will cause LLDP to be `active`, however, no information will be shared.

![Step01 - LLDP Evidence](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_LLDP_evidence.png)  

[Step01 - LLDP setup](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_LLDP.png)

---

## 5. Security Policy & NAT

*The strict ordering is required to allow infrastructure while blkcing lateral movement.*

The naming from the table below is for a general understand, the image is the exact configuration.

| No. | Rule Name       | Source      | Destination | Service      | Action   |
|-----|-----------------|-------------|-------------|--------------|----------|
| 1   | Infrastructure  | Any         | Any         | DHCP, DNS    | Accept   |
| 2   | Allow Printing  | Client_Nets | Printer_Net | ICMP, SMB    | Accept   |
| 3   | Blocked Lateral | Client_Nets | Client_Nets | Any          | **Drop** |
| 4   | Internet        | Any         | Any         | HTTP/S, ICMP | Accept   |
| 5   | Cleanup         | Any         | Any         | Any          | Drop     |  

![Step01 - Rules](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_Rules.png)

### 5.1 NAT

Called NAT for "Supernet".
Used Automatic-NAT, with hiding behind the firewall.

Address used was 10.47.0.0/16. General and short.

![Step01 - NAT](/Cisco-Checkpoint-Segmentation-Lab/Images/Step01_NAT.png)
