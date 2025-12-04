# IP Address Plan

## Addressing Overview

| Segment       | VLAN | Subnet          | Gateway (FW)     | Purpose               |
|---------------|------|-----------------|------------------|-----------------------|
| **WAN**       | N/A  | 192.168.40.0/24 | .1 (Home Router) | NAT to Internet       |
| **Mgmt**      | 199  | 10.47.199.0/24  | .1               | Management            |
| **Clients A** | 101  | 10.47.39.0/24   | .1               | Clients on SW01       |
| **Clients B** | 102  | 10.47.40.0/24   | .1               | Clients on SW02       |
| **Clients C** | 103  | 10.47.41.0/24   | .1               | Clients on SW03       |
| **Printers**  | 170  | 10.47.170.0/24  | .1               | Shared Print Services |

## DHCP Configuration
The Checkpoint Firewall acts as the DHCP Server for all Client VLANs.
**Gateway Option:** 10.47.x.1
**DNS:** 192.168.40.1