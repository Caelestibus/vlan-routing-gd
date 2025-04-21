# 🚀 Inter-VLAN Routing with Router-on-a-Stick (Cisco Packet Tracer Lab)

This project demonstrates how to configure inter-VLAN communication using a single router interface with subinterfaces (Router-on-a-Stick) in **Cisco Packet Tracer**.

---

## 📌 Description

In this lab, I set up a network environment that uses VLANs to segment traffic and enable routing between VLANs via a router. This simulates a real-world scenario where different departments in an organization (Accounting and IT) are separated but need to communicate securely.

---

## 📁 Network Topology

- **Router:** Single interface (GigabitEthernet 0/0) with subinterfaces
- **Switch:** Handles VLAN 10 and VLAN 20
- **Devices:**
  - `PC1` - VLAN 10 - 192.168.10.2
  - `PC2` - VLAN 10 - 192.168.10.3
  - `PC3` - VLAN 20 - 192.168.20.2
  - `PC4` - VLAN 20 - 192.168.20.3

---

## ⚙️ Router Configuration

```bash
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

---


🧾 Switch Configuration

bash
Switch> enable
Switch# configure terminal

Switch(config)# vlan 10
Switch(config-vlan)# name Accounting
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name IT
Switch(config-vlan)# exit

Switch(config)# interface range Fa0/1 - 2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

Switch(config)# interface range Fa0/3 - 4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# no shutdown
Switch(config-if)# exit

---


💻 PC IP Address Table

PC	VLAN	IP Address	Subnet Mask
PC1	10	192.168.10.2	255.255.255.0
PC2	10	192.168.10.3	255.255.255.0
PC3	20	192.168.20.2	255.255.255.0
PC4	20	192.168.20.3	255.255.255.0

---


🧪 Ping Test Results

From PC1:

bash
ping 192.168.10.3   ✅
ping 192.168.20.3   ❌ (Timed out)

Cause: Trunk port not configured on switch.
Fix: Trunk the switch port connected to the router.

Trunk Fix:

bash
Switch> enable
Switch# configure terminal
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# no shutdown
Retest From PC1:

bash
ping 192.168.20.2   ✅
From PC3:

bash
ping 192.168.20.3   ✅
ping 192.168.10.2   ✅


---

🧠 Lessons Learned

Subinterfaces must use the correct VLAN ID in the encapsulation dot1Q command.

Switch ports connected to the router must be in trunk mode.

Access ports must be assigned to the correct VLANs.

Inter-VLAN routing requires proper IP addressing and subnet matching.



👨‍💻 Author
Lawuru David Weyinmi
💼 Cybersecurity | 📚 Packet Tracer Enthusiast


✅ Status
✅ Working | 📡 Ping test successful across VLANs | 🔁 VLAN Routing Operational


---










