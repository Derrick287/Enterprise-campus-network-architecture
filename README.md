# Enterprise-campus-network-architecture
Production-grade enterprise network architecture implementing a resilient hierarchical Core-Access design with Layer 3 inter-VLAN routing, EtherChannel redundancy, Cisco ASA security enforcement, centralized AD-managed DHCP/DNS services, and Cisco CME VoIP infrastructure for secure, scalable, and highly available multi-department operations.


Enterprise Campus Network Architecture: Secure Multi-Service Infrastructure

1. Project Vision and Business Objective

In the modern enterprise landscape, a hierarchical network design is a strategic mandate for ensuring organizational resilience. By implementing a structured, multi-tier architecture, we move beyond simple connectivity to a framework where scalability, security, and service availability are inherent to the design. This architecture addresses the complex requirements of a diverse departmental environment, providing the high-speed backbone necessary for data-intensive operations while maintaining the isolation required for compliance and risk management.

Core Business Objective The primary objective of this architecture is to deliver a resilient enterprise network infrastructure. This design ensures secure departmental communication, robust centralized services, integrated Unified Communications (VoIP), and a modular framework prepared for rapid organizational expansion.

Design Philosophy

* Security-First Design: Enforcing a "Zero-Trust" posture at both the perimeter and internal boundaries through granular VLAN segmentation and stateful Access Control Lists (ACLs).
* Operational Continuity: Maximizing uptime through link aggregation (EtherChannel) and structured Layer 3 routing to eliminate single points of failure.
* Unified Communications: Prioritizing voice traffic through dedicated segmentation, ensuring high-fidelity communication without impacting standard data operations.

This philosophy ensures that technical configurations remain aligned with the ultimate business goals of productivity and infrastructure integrity.


--------------------------------------------------------------------------------


2. Core Engineering Design Goals

Strategic business objectives are translated into technical reality through specific engineering goals. By aligning these goals with organizational impact, we create a high-performance environment where every configuration serves a tactical purpose.

Engineering Goals vs. Strategic Impact

Engineering Goal	Strategic Impact
High Availability	Deployment of redundant EtherChannel uplinks and Layer 3 SVIs ensures link and gateway redundancy, drastically reducing the blast radius of hardware failures.
Secure Departmental Isolation	VLAN-based segmentation and Firewall-enforced ACLs prevent unauthorized lateral movement, protecting sensitive departmental data.
Centralized Management	Integration of DNS and DHCP within an Active Directory environment provides a single source of truth for identity and IP management, reducing administrative overhead.
Reliable VoIP Deployment	Dedicated Voice VLANs and Cisco CME (SCCP) ensure low-latency, high-priority voice traffic, delivering professional-grade communication services.

These goals establish a foundation where network services are transparent to the user yet robust enough to support critical enterprise functions.


--------------------------------------------------------------------------------


3. Technologies and Infrastructure Stack

The hardware and protocol stack was selected based on enterprise-grade standards to ensure interoperability and long-term supportability.

Infrastructure Components:

* Hardware: The core is anchored by a Cisco Catalyst 3650 (Core_Switch) providing high-speed multilayer switching. Perimeter defense is managed by a Cisco ASA 5506-X (Firewall), and edge routing/telephony is handled by a Cisco 2811 (Router).
* L2/L3 Networking: Bandwidth aggregation and link redundancy are achieved via EtherChannel (LACP) using the channel-group mode active command. Traffic is managed through Inter-VLAN Routing, Layer 3 SVIs, and STP PortFast for rapid port convergence on access-layer devices.
* Security & Gateway: Edge protection utilizes Dynamic NAT (PAT) on the ASA to translate internal scopes to the outside interface IP (200.0.0.2). Traffic flow is governed by Extended ACLs (GUEST_BLOCK, IT_ACCESS) and Static Routing for deterministic path selection.
* Unified Communications: Voice services are powered by Cisco CallManager Express (CME) utilizing the Skinny Client Control Protocol (SCCP) for handset signaling and ephone-dn for directory management.
* Core Services: Identity and addressing are managed via Active Directory Integrated DNS/DHCP.

This integrated stack creates a unified fabric capable of supporting diverse enterprise applications with maximum efficiency.


--------------------------------------------------------------------------------


4. Network Segmentation and VLAN Architecture

Adhering to the principle of "Least Privilege," this architecture utilizes VLAN segmentation to isolate broadcast domains and limit the scope of potential network threats.

VLAN Assignment Matrix

VLAN ID	Name	Network Address Scope	Default Gateway
10	StaffWireless	192.168.10.100 /22	192.168.10.1
20	DEPTC	192.168.20.100 /24	192.168.20.1
21	IT	192.168.21.100 /24	192.168.21.1
30	PPRM	192.168.30.100 /24	192.168.30.1
40	RME	192.168.40.100 /24	192.168.40.1
50	PEA	192.168.50.100 /24	192.168.50.1
60	Servers	192.168.60.0 /24	192.168.60.1
70	GuestWireless	172.16.0.100 /22	172.16.0.1
80	Voice	192.168.80.100 /24	192.168.80.1

A critical efficiency in this design is the use of ip helper-address 192.168.60.30 configured on the Core_Switch SVIs. This configuration relays DHCP requests from isolated departmental VLANs to the centralized Active Directory DHCP server, ensuring consistent IP provisioning across the entire campus.


--------------------------------------------------------------------------------


5. Security Policy and Edge Protection

The Cisco ASA 5506-X acts as the "Zero-Trust" perimeter, meticulously inspecting all traffic entering the corporate backbone and enforcing strict inter-zone policies.

Firewall Configuration Synthesized:

* Port Address Translation (PAT): Using nat (inside,outside) dynamic interface, the firewall enables multiple internal hosts across the GUEST and INSIDE-NET objects to share the single public IP address of the ASA outside interface (200.0.0.2), providing both address conservation and internal topology obfuscation.
* Access Control Policies: The GUEST_BLOCK ACL is the primary mechanism for isolating the 172.16.0.0/22 range. It specifically denies traffic to the corporate 192.168.0.0/16 backbone while permitting internet-bound traffic, preventing guest devices from performing reconnaissance on internal assets.
* Traffic Inspection: The global_policy facilitates deep packet inspection for DNS, FTP, ICMP, and TFTP. This ensures that protocol-specific attacks are mitigated at the edge before they can penetrate the internal network.


--------------------------------------------------------------------------------


6. Unified Communications: VoIP Integration

Converging voice and data traffic onto a single infrastructure requires careful orchestration to prevent voice degradation. This architecture leverages dedicated Voice VLANs to isolate sensitive RTP traffic.

Cisco CME Configuration: The 2811 Router serves as the telephony gateway with the following parameters:

* Source Address: 10.10.10.10 (Loopback0) on Port 2000.
* Directory Assignments:
  * 1001: MAC 0060.3E13.8442 (ephone 1)
  * 1002: MAC 0001.C996.8442 (ephone 2)
  * 1003: MAC 00E0.8F6E.5E81 (ephone 3)
  * 1004: MAC 00D0.58E5.578D (ephone 4)

The "So What?" of Implementation: The application of switchport voice vlan 80 on access switches (such as SW0-PPRM) is a critical design choice. This command utilizes CDP/LLDP-MED to signal the IP phone to tag its own traffic on VLAN 80 while allowing the attached PC to remain untagged on the data VLAN (e.g., VLAN 30). This allows for differentiated Quality of Service (QoS) and ensures that high data utilization on a workstation does not compromise the clarity of a voice call.


--------------------------------------------------------------------------------


7. Centralized Services: DNS and DHCP Management

Centralized management of name resolution and IP addressing through Active Directory is essential for maintaining a predictable and manageable network environment.

Internal DNS Resolution (ncpd.local): The AD DNS server maintains the following critical A-Records to ensure resource accessibility:

* ad / dhcp / dns.ncpd.local: 192.168.60.30
* bio.ncpd.local: 192.168.60.50
* lib.ncpd.local: 192.168.60.60
* print.ncpd.local: 192.168.60.40 (Centralized Print Server)

DHCP Service Architecture: IP distribution is automated via the serverPool (192.168.60.0/24) for infrastructure assets and departmental pools for end-users. This systematic approach, combined with DHCP relaying on the Core_Switch, ensures that devices receive the correct gateway and DNS information regardless of their physical point of attachment.


--------------------------------------------------------------------------------


8. Operational Validation and Verification

Verification is the final phase of the engineering lifecycle, confirming that the deployed configuration matches the design intent and meets performance standards.

Verification Command Reference

# --- CORE_SWITCH AUDIT ---
# Validate LACP EtherChannel status and member port health
show etherchannel summary

# Verify VLAN database and port-to-VLAN mapping
show vlan brief

# Audit Layer 3 routing table for reachability to ASA and Router
show ip route

# --- ASA FIREWALL AUDIT ---
# Verify stateful security policy enforcement
show access-lists

# --- ROUTER / CME AUDIT ---
# Confirm registration and MAC binding for VoIP handsets
show ephone registered



--------------------------------------------------------------------------------


9. Key Engineering Achievements and Roadmap

This implementation delivers a production-grade campus environment that effectively balances high-performance throughput with a rigorous security posture.

Key Engineering Achievements:

* Redundant LACP Uplinks: Established resilient, high-bandwidth trunking between the Core and Access layers to eliminate throughput bottlenecks.
* Secure Guest Isolation: Leveraged ASA stateful inspection and ACLs to provide internet access to guests without compromising the corporate 192.168.0.0/16 backbone.
* Layer 3 SVI Centralization: Optimized internal routing by terminating VLANs on the Core_Switch, reducing "router-on-a-stick" latency.
* STP PortFast Optimization: Reduced workstation connectivity delays by bypassing standard STP listening/learning states on edge ports.

Future Roadmap:

* Dynamic Routing Migration: Transitioning from static routes to OSPF for automated path discovery and faster convergence.
* First-Hop Redundancy: Implementing HSRP to provide gateway redundancy for departmental segments.
* Enterprise AAA: Integration of RADIUS/TACACS+ for centralized administrative authentication and audit logging.
* Next-Gen Ready: Developing an IPv6 transition plan and exploring SD-WAN for enhanced branch office connectivity.


--------------------------------------------------------------------------------


Designed and implemented by Senior Enterprise Network Architect | Enterprise Network Engineering Portfolio

