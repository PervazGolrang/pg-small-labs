# Step 06 - Validation

At the final step is validation of the Zero Trust Access Layer design, for mitigation of lateral movement within a corporate network. 

---

# 1. Security Validation (East-West traffic)

## Test Overview:

| Soirce | Destination | Expected Result | Logic / Rule                                           |
|--------|-------------|-----------------|--------------------------------------------------------|
| PC1    | PC2         | FAIL            | Blocked by Firewall Rule 3 (Inter-Switch Segmentation) |
| PC1    | PC3         | FAIL            | Blocked by Firewall Rule 3 (Inter-Switch Segmentation) |
| PC1    | Mgmt        | FAIL            | Blocked by Cleanup Rule (Network Segmentation)         |
| PC1    | 8.8.8.8     | PASS            | Allowed by Firewall Rule 4 (Internet Access + NAT)     | 
| PC1    | Printer     | PASS            | Allowed by Firewall Rule 2 (Print Services)            |
| Mgmt   | PC1         | PASS            | Allowed by Admin/Management Policy                     |

**From PC1 (10.47.40.10):**

```bash
# Attempt to ping PC2 on a different switch (VLAN 101)
ping -c 4 10.47.39.3
# Result: 100% packet loss (Traffic dropped by FW)
!
# Attempt to ping PC3 on a different switch (VLAN 103)
ping -c 4 10.47.41.3
# Result: 100% packet loss (Traffic dropped by FW)
!
# Attempt to ping Management Station (VLAN 199)
ping -c 4 10.47.199.3
# Result: 100% packet loss (Traffic dropped by FW)
```

![Step06 - From PC1 FAIL](/Cisco-Checkpoint-Segmentation-Lab/Images/step06_pc1.png)

---

## 2. Functional Validation (Business Services)

Confirm that security controls do not impede legitimate business workflows.

**From PC1 (10.47.40.10):**

```bash
# Test Internet Connectivity
ping -c 4 8.8.8.8
# Result: 0% packet loss (Reply received)

# Test Connectivity to Printer (VLAN 170)
ping -c 4 10.47.170.3
# Result: 0% packet loss (Reply received)
```

![Step06 - From PC1 PASS](/Cisco-Checkpoint-Segmentation-Lab/Images/Step06_pc_pass.png)

---

# 3. Operational Validation (Management Access)

Confirm that IT Administrators can still reach endpoints for support and troubleshooting.

**From Mgmt Node (10.47.199.3):**

```bash
# Test reachability to Client PC1
ping -c 4 10.47.40.3
# Result: 0% packet loss (Reply received)
```

![Step06 - Management PASS](/Cisco-Checkpoint-Segmentation-Lab/Images/Step06_management.png)
