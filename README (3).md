# VLAN Implementation Lab

A Cisco Packet Tracer lab demonstrating the configuration of VLANs on a switch and enabling Inter-VLAN communication using Router-on-a-Stick.

**Experiment No:** 3
**Course:** Cyber Security (CEH)

📄 Full report: [`project_3_VLAN_Implementation_Lab.docx`](./project_3_VLAN_Implementation_Lab.docx) · [PDF](./project_3_VLAN_Implementation_Lab.pdf)

🔧 Tool used: [Cisco Packet Tracer](https://www.netacad.com/articles/news/download-cisco-packet-tracer)

---

## 📌 Objective
Configure VLANs on a switch, assign ports to different VLANs, and enable communication between VLANs using Router-on-a-Stick configuration.

## 📖 Theory
A Virtual Local Area Network (VLAN) is a logical grouping of devices within a network regardless of physical location. VLANs improve network performance, enhance security, and simplify management by separating broadcast domains. Inter-VLAN communication requires a Layer 3 device such as a router or Layer 3 switch.

**Benefits of VLANs:**
- Improved security
- Reduced broadcast traffic
- Better network management
- Logical segmentation of users and departments

## 🌐 Network Topology

| VLAN | Department | Device | IP Address |
|------|-----------|--------|------------|
| 10 | SALES | PC1 | 192.168.10.11 |
| 10 | SALES | PC2 | 192.168.10.12 |
| 20 | HR | PC3 | 192.168.20.11 |
| 20 | HR | PC4 | 192.168.20.12 |

**Gateway Addresses**
- VLAN 10 Gateway: `192.168.10.1`
- VLAN 20 Gateway: `192.168.20.1`

**Devices Used:** 1 Cisco Switch, 1 Cisco Router, 4 PCs, Ethernet Cables

## ⚙️ Procedure

**Step 1: Create VLANs**
```
enable
configure terminal

vlan 10
name SALES

vlan 20
name HR
end
```

**Step 2: Assign Ports to VLANs**
```
! VLAN 10
interface range fa0/1 - 2
switchport mode access
switchport access vlan 10

! VLAN 20
interface range fa0/3 - 4
switchport mode access
switchport access vlan 20
```

**Step 3: Configure Trunk Port**
```
interface fa0/24
switchport mode trunk
```

**Step 4: Configure Router-on-a-Stick**
```
enable
configure terminal

interface g0/0
no shutdown

interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
end
```

**Step 5: Configure PC IP Addresses** — assigned as shown in the topology table above, with each PC's gateway set to its VLAN's router sub-interface.

## 🧪 Testing

**Before Inter-VLAN Routing**
VLAN 10 and VLAN 20 are separate broadcast domains — devices in the same VLAN can communicate, but devices in different VLANs cannot.

| Ping | Result |
|------|--------|
| PC1 (192.168.10.11) → PC2 (192.168.10.12) | ✅ Success |
| PC1 (192.168.10.11) → PC4 (192.168.20.11) | ❌ Failed |

**After Inter-VLAN Routing** (via Router-on-a-Stick)

| Ping | Result |
|------|--------|
| PC1 (VLAN 10) → PC4 (VLAN 20) | ✅ Success |
| PC2 (VLAN 10) → PC5 (VLAN 20) | ✅ Success |

## ✅ Verification Commands
```
show vlan brief
show interfaces trunk
show running-config
show ip interface brief
```

## 🔍 Observations
1. VLAN 10 and VLAN 20 were successfully created.
2. Ports were assigned correctly to their respective VLANs.
3. Devices within the same VLAN communicated successfully.
4. Devices in different VLANs could not communicate before router configuration.

## 📊 Results
The VLAN implementation was completed successfully. VLAN 10 and VLAN 20 were created and configured on the switch, and Inter-VLAN routing was established using a router, allowing all four PCs to communicate across VLAN boundaries.

## 🏁 Conclusion
This lab demonstrated the creation and management of VLANs in a switched network. VLANs effectively separated network traffic into different broadcast domains, and Router-on-a-Stick configuration enabled communication between VLANs — illustrating the importance of Layer 3 devices in segmented networks.

## 🛠️ Skills Demonstrated
- VLAN creation and port assignment on Cisco switches
- Trunk port configuration
- Router-on-a-Stick (sub-interface) configuration
- Inter-VLAN routing
- Network verification and troubleshooting via CLI
