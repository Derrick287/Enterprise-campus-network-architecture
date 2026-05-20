# Enterprise Core–Access Infrastructure

Production-grade enterprise network architecture implementing a resilient hierarchical Core–Access design with Layer 3 inter-VLAN routing, EtherChannel redundancy, Cisco ASA security enforcement, centralized AD-managed DHCP/DNS services, and Cisco CME VoIP infrastructure for secure, scalable, and highly available multi-department operations.

---

# 📌 Project Overview

This project demonstrates the design and implementation of a secure and resilient enterprise network infrastructure based on Cisco networking technologies. The environment was architected using a hierarchical Core–Access model to ensure scalability, redundancy, simplified management, and optimized traffic flow across multiple organizational departments.

The infrastructure integrates:

* Layer 3 inter-VLAN routing
* Cisco ASA perimeter security
* VLAN segmentation
* Dynamic NAT
* EtherChannel redundancy
* VoIP telephony services
* Centralized DHCP/DNS services
* Active Directory integration
* Enterprise endpoint connectivity
* Network performance optimization

The project simulates a real-world enterprise environment supporting voice, data, centralized services, and secure external connectivity.

---

# 🏗️ Network Architecture

## Hierarchical Core–Access Design

The infrastructure follows a two-tier enterprise architecture:

### Core Layer

Handled by a Cisco Layer 3 Core Switch responsible for:

* Inter-VLAN routing
* Traffic aggregation
* Default gateway services
* High-speed switching
* EtherChannel aggregation
* Network segmentation

### Access Layer

Departmental access switches provide:

* Endpoint connectivity
* Voice VLAN access
* PortFast-enabled interfaces
* Trunk uplinks to the Core
* Department isolation

---

# 🔐 Security Architecture

Security is enforced using a Cisco ASA firewall positioned between the internal enterprise network and external connectivity.

## Security Features

### VLAN Segmentation

Logical isolation between departments:

| VLAN    | Department     |
| ------- | -------------- |
| VLAN 10 | Staff_Wireless |
| VLAN 20 | DeptC          |
| VLAN 21 | IT             |
| VLAN 30 | PPRM           |
| VLAN 40 | RME            |
| VLAN 50 | PEA            |
| VLAN 60 | Servers        |
| VLAN 70 | Guest_Wireless |
| VLAN 80 | Voice          |

---

### Cisco ASA Firewall

Implemented:

* Security zones
* ACL enforcement
* Dynamic NAT
* Guest isolation
* Traffic inspection
* Controlled inter-network communication

### Key Security Policies

#### Guest Isolation

Guests on `172.16.0.0/22` are blocked from accessing internal corporate networks.

```bash
access-list GUEST_BLOCK extended deny ip 172.16.0.0 255.255.252.0 192.168.0.0 255.255.0.0
access-list GUEST_BLOCK extended permit ip any any
```

#### IT Administrative Access

Dedicated IT subnet granted privileged access for management and troubleshooting.

---

# 📡 Redundancy & High Availability

## EtherChannel (LACP)

Implemented aggregated uplinks between the Core Switch and departmental access switches using LACP Port-Channels.

### Benefits

* Increased bandwidth
* Link redundancy
* Load balancing
* Fault tolerance

If one physical uplink fails, connectivity remains active through remaining links.

---

# ☎️ VoIP Infrastructure

The enterprise network includes a fully integrated Cisco CME (Call Manager Express) telephony environment.

## VoIP Components

* Dedicated Voice VLAN (VLAN 80)
* Cisco 2811 CME Router
* IP Phone auto-registration
* TFTP-based provisioning
* Extension assignment
* Voice/data separation

## Implemented Extensions

| Extension | Purpose      |
| --------- | ------------ |
| 1001      | Office Phone |
| 1002      | Office Phone |
| 1003      | Office Phone |
| 1004      | Office Phone |
| 1005      | Office Phone |
| 1006      | Office Phone |
| 1007      | Office Phone |
| 1008      | Office Phone |

---

## CME Configuration Example

```bash
telephony-service
 max-ephones 10
 max-dn 10
 ip source-address 192.168.80.254 port 2000
 auto assign 1 to 8
```

---

# 🌐 Centralized Services

An Active Directory server provides centralized enterprise services.

## Services Provided

### DHCP

Dynamic IP assignment across all VLANs.

### DNS

Internal name resolution using:

```text
.ncpd.local
```

### User & Resource Management

Managed centrally using Active Directory.

---

# ⚡ Performance Optimization

## Implemented Enhancements

### STP PortFast

Enabled on all endpoint-facing interfaces to eliminate unnecessary startup delays.

### VLAN Traffic Isolation

Reduced broadcast domains and improved performance.

### Layer 3 Routing

Efficient inter-department communication handled directly at the Core.

---

# 🛠️ Technologies Used

| Category            | Technologies                |
| ------------------- | --------------------------- |
| Routing & Switching | Cisco 3650, Cisco 2811      |
| Firewall            | Cisco ASA 5506-X            |
| VoIP                | Cisco CME                   |
| Redundancy          | EtherChannel (LACP)         |
| Routing             | Inter-VLAN Routing          |
| Security            | ACLs, NAT                   |
| Services            | DHCP, DNS, Active Directory |
| Simulation          | Cisco Packet Tracer         |

---

# 📂 Repository Structure

```text
Enterprise-Core-Access-Infrastructure/
│
├── README.md
├── topology/
├── configs/
├── services/
├── security/
├── documentation/
├── monitoring/
└── assets/
```

---

# 🚀 Deployment Sequence

## Phase 1 — Core Infrastructure

* Configure VLANs
* Configure SVIs
* Enable IP routing
* Configure EtherChannels

## Phase 2 — Access Layer

* Configure trunk links
* Configure access ports
* Configure Voice VLANs
* Enable PortFast

## Phase 3 — Security

* Configure ASA interfaces
* Configure NAT
* Apply ACLs
* Enforce segmentation

## Phase 4 — Centralized Services

* Configure DHCP scopes
* Configure DNS records
* Configure helper addresses

## Phase 5 — VoIP

* Configure CME
* Configure ephones
* Generate CNF files
* Register IP phones

---

# 🔍 Troubleshooting & Validation

## Validation Commands

### Core Switch

```bash
show vlan brief
show etherchannel summary
show ip route
```

### ASA Firewall

```bash
show access-list
show nat
show route
```

### CME Router

```bash
show ephone registered
show running-config
```

---

# 📈 Key Outcomes

* Improved enterprise scalability
* Secure departmental isolation
* High availability through redundancy
* Centralized service management
* Integrated enterprise VoIP
* Optimized endpoint connectivity
* Simplified troubleshooting and administration

---

# 🎯 Learning Objectives Demonstrated

This project demonstrates practical implementation skills in:

* Enterprise Network Design
* Cisco Routing & Switching
* Network Security
* Firewall Administration
* VoIP Infrastructure
* VLAN Segmentation
* DHCP/DNS Integration
* Network Redundancy
* Infrastructure Troubleshooting

---

# 📜 License

This project is intended for educational, research, and enterprise network architecture demonstration purposes.

---

# 👨‍💻 Author

Designed and implemented by *Derrick Nyongesa Wesonga* an ICT & Network Infrastructure professional specializing in enterprise systems, VoIP infrastructure, and resilient Cisco-based architectures.

