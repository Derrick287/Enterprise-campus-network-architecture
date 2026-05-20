---

### 🌐 Edge Router Interface Allocations


| Interface ID | Description/Zone | IP Address | Subnet Mask |
| :--- | :--- | :--- | :--- |
| **Loopback0** | Management/SCCP Source | `10.10.10.10` | `255.255.255.255` |
| **FastEthernet0/0** | External (Internet) | `8.8.8.1` | `255.255.255.0` |
| **FastEthernet0/1** | Boundary Link to ASA | `200.0.0.1` | `255.255.255.252` |

---

### 🛡️ Cisco ASA Firewall Interface Allocations


| Interface ID | Description/Zone | IP Address | Subnet Mask |
| :--- | :--- | :--- | :--- |
| **GigabitEthernet1/1** | Outside Zone | `200.0.0.2` | `255.255.255.252` |
| **GigabitEthernet1/2** | Inside Zone | `10.0.0.2` | `255.255.255.252` |

---

### 🎛️ Core Switch Interface Allocation Table


| Interface | Target Destination | Mode | VLAN / Channel-Group |
| :--- | :--- | :--- | :--- |
| **Gi1/0/1** | Firewall (Inside Zone) | Routed (L3) | IP: `10.0.0.1 255.255.255.252` |
| **Gi1/0/2–5** | Internal Servers | Access | VLAN 60 |
| **Gi1/0/6** | Staff Wireless AP | Access | VLAN 10 |
| **Gi1/0/7** | Guest Wireless AP | Access | VLAN 70 |
| **Gi1/0/8–9** | SW0-PPRM | Trunk (LACP) | Channel-group 1 mode active |
| **Gi1/0/10–11** | SW1-RME | Trunk (LACP) | Channel-group 2 mode active |
| **Gi1/0/12–13** | SW2-PEA | Trunk (LACP) | Channel-group 3 mode active |
| **Gi1/0/14–15** | SW3-C_IT | Trunk (LACP) | Channel-group 4 mode active |
| **Gi1/0/16** | RESERVED-EXPANSION | N/A | Shutdown |
| **Gi1/0/17** | RESERVED-EXPANSION | N/A | Shutdown |

---

### 🔌 SW0-PPRM Port-Map (Standard Template)


| Port Range | VLAN Type | VLAN ID | Service/Device |
| :--- | :--- | :--- | :--- |
| **Fa0/1** | Data | 30 | Departmental Printer |
| **Fa0/2–3** | Data & Voice | 30, 80 | IP Phone and Desktop PC |
| **Fa0/4–20** | Data | 30 | Desktop PC / Endpoints |
| **Gi0/1–2** | Trunk | 30, 80 | Uplink to Core (Channel-Group 1) |

---




