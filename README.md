
# **DTP & VTP Configuration**

## **Overview**
This lab focuses on configuring VLANs, trunk ports, and VLAN Trunking Protocol (VTP) across multiple switches. The goal is to:
- Set up VLANs and ensure proper propagation.
- Disable Dynamic Trunking Protocol (DTP) to enhance security.
- Verify VLAN assignments and trunk configurations.

## **Network Topology**
![image](https://github.com/user-attachments/assets/e1eceeda-44ab-4dcd-b13f-656ac15252e3)


## **VLAN Configuration**
| VLAN | Name   | Subnet        |
|------|--------|--------------|
| 10   | VLAN10 | 10.0.0.0/26  |
| 20   | VLAN20 | 10.0.0.64/26 |
| 30   | VLAN30 | 10.0.0.128/26 |
| 40   | VLAN40 | 10.0.0.192/26 |

---

## **Configuration Steps**

### **SW1 (VTP Server)**
```bash
# Set the VTP domain and mode
vtp domain CCNA
vtp mode server

# Create VLANs
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30

# Configure trunk ports and disable DTP
interface range gigabitEthernet 0/1-2
 switchport mode trunk
 switchport nonegotiate
```

---

### **SW2 (VTP Transparent)**
```bash
# Set VTP mode to transparent
vtp mode transparent

# Manually add VLAN 40 (won’t propagate)
vlan 40
 name VLAN40

# Configure trunk ports and disable DTP
interface range gigabitEthernet 0/1-2
 switchport mode trunk
 switchport nonegotiate
```

---

### **SW3 (VTP Client)**
```bash
# Set VTP mode to client
vtp mode client

# Configure trunk ports and disable DTP
interface gigabitEthernet 0/1
 switchport mode trunk
 switchport nonegotiate

# Assign VLANs to access ports
interface range fastEthernet 0/2-3
 switchport mode access
 switchport access vlan 30
```

---

## **Verification Commands**
Use these commands to verify your configurations:

| Command | Description |
|---------|------------|
| `show interfaces trunk` | Check trunk port status |
| `show vtp status` | Verify VTP mode and domain |
| `show vlan brief` | Display VLAN assignments |
| `show dtp` | Confirm DTP is disabled |

---

## **Troubleshooting & Fixes**
### **Issue: Spanning Tree Blocking on Trunk Ports**
If you see messages like this:
```
%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking GigabitEthernet0/1 on VLAN0001. Inconsistent port type.
```
Try resetting the trunk port:
```bash
interface gigabitEthernet 0/1
shutdown
no shutdown
```
Then recheck with:
```bash
show spanning-tree inconsistentports
```

---

## **Files in This Repo**
- **`Day 19 Lab - DTP & VTPprogress.pkt`** → Packet Tracer file with full configurations  
- **`README.md`** → This documentation  

---

## **Notes**
- **VLAN 40 will not propagate** because SW2 is in transparent mode.
- ![dtp](https://github.com/user-attachments/assets/ddceb34c-c26f-4908-bc0e-4f790e4ad374)

- **DTP is disabled** to prevent unwanted trunk negotiations.
- **VTP client mode does not allow VLAN creation** (as seen when trying to add VLAN 50 on SW3).
![image](https://github.com/user-attachments/assets/a2c506d4-8fe0-415e-9912-ec205a30c98e)



---

### **How to Use This Lab**
1. Open `Day 19 Lab - DTP & VTPprogress.pkt` in **Cisco Packet Tracer**.
2. Verify VLAN configurations using `show vlan brief`.
3. Test VTP propagation across switches with `show vtp status`.
4. Ensure trunk links are correctly established using `show interfaces trunk`.
5. Experiment with adding VLANs on different switches to test VTP behavior.
