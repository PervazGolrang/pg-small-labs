## Step05 - Endpoint configuration

Alpine Linux is used to simulate endpoints, including Printer. Since these are lightweight containers, they require specific commands to handle networking and routing tables correctly.

---

Username: `cisco`
Password: `cisco`

If everything is set up correctly, this should recieve an IP address.

## 1. Setting up DHCP
```bash
sudo udhcpc
```

![Step05 - DHCP Discover](/Cisco-Checkpoint-Segmentation-Lab/Images/Step05_DHCP.png)

It is sucessfull.

---

## 2. Manual Gateway

The Gateway has to be setup manually, without this, the PC cannot route to Internet or Printers.

```bash
## PC1
sudo ip route add default via 10.47.40.1
!
## PC2
sudo ip route add default via 10.47.39.1
!
## PC3
sudo ip route add default via 10.47.41.1
!
## Printer
sudo ip route add default via 10.47.170.1
!
## Mgmt
sudo ip route add default via 10.47.199.1
```
