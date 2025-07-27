# Cisco-ASA-Firewall-Config

## Objective
The objective of this project was to design and implement a secure, multi-site network topology using Cisco routers, switches, and an ASA 5506 firewall. The implementation included dynamic routing using both OSPF and EIGRP, VLAN segmentation, DHCP services, remote device access via SSH, and secure outbound connectivity to a simulated Internet through the ASA firewall. Access control policies were configured to restrict interdepartmental traffic and secure access to sensitive network resources.

### Skills Learned
- Secure network design
- VLAN implementation and trunking
- DHCP configuration and verification
- Dynamic routing using EIGRP
- ASA 5506 firewall configuration
- Standard and extended ACLs
- SSH configuration for secure device access
- Network segmentation and access control
- Connectivity and route verification

  ### Tools Used
- Cisco Packet Tracer
- Cisco 4331 Routers
- Cisco 3560 Multilayer Switches
- ASA 5506-X Firewall
- WRT300N Wireless Router
- Windows PCs and network printers
- Terminal CLI (SSH, ping, show commands)

## Project Outcome 
Successfully deployed a secure, segmented network with dynamic EIGRP routing, VLANs, DHCP services, and SSH-secured remote access. Implemented ACLs to control interdepartmental traffic and configured the ASA firewall to enable Internet connectivity, verifying full network functionality and policy enforcement.

## Steps
### Phase One: Core Network Configuration 
<img width="1208" height="770" alt="image" src="https://github.com/user-attachments/assets/3e103815-eedd-4828-b23e-5d7bf17553f8" />
*Network Topology*

#### IP Addressing Scheme 
<img width="628" height="519" alt="image" src="https://github.com/user-attachments/assets/6d69372a-d670-4888-87c6-70fa890048a3" />

#### Assigned IP addresses and subnets based on the IP schema 
<img width="719" height="148" alt="image" src="https://github.com/user-attachments/assets/14291bb2-6fc7-4fb2-adc4-4548093da0f8" />

*Router1 Interface Brief*

<img width="779" height="231" alt="image" src="https://github.com/user-attachments/assets/285b2669-d8c8-442c-bae3-d1965106aff9" />

*Router2 Interface Brief*

- Set up subinterfaces on routers for each VLAN

<img width="670" height="247" alt="image" src="https://github.com/user-attachments/assets/9fb2e5a0-974a-45ab-9ff6-e85fcc0e9215" />

*Router3 Interface Brief*

- Set up subinterfaces on routers for each VLAN

#### VLAN and DHCP Configuration
#### MLSW1
- Assigned ports to VLANs for Sales/Marketing, Finance/Operations, and TACACS+ servers
- Connected to R2 via trunk for inter-VLAN routing
- Servers configured with static IP addresses
<img width="692" height="279" alt="image" src="https://github.com/user-attachments/assets/29f5c828-f7cd-470d-8719-ce568cde72bf" />

*MLSW1 Vlan Brief*

#### MLSW2 
- Access switch for user devices
- Ports assigned to appropriate VLANs (e.g., Sales, Marketing)
- Trunked to SW3 to carry VLAN traffic upstream

  <img width="686" height="325" alt="image" src="https://github.com/user-attachments/assets/7ba8b001-1752-42db-915e-72efdfa8f755" />

*MLSW2 Vlan Brief*

#### MLSW3 and Router3
- Central switch for client VLANs
- Trunked to R3
- Subinterfaces created on R3 for each VLAN
- DHCP pools configured on R3 to assign IPs to VLAN clients
- Verified IP assignments and VLAN segmentation
  
<img width="721" height="312" alt="image" src="https://github.com/user-attachments/assets/70272643-49f2-433d-9e38-6891f10e77ce" />

*MLSW3 Vlan Brief*

<img width="394" height="345" alt="image" src="https://github.com/user-attachments/assets/5e9799ce-0abf-46d3-a942-629d82e4a535" />

 *Running-config Router3*

 ### Phase Two: Routing Configuration (OSPF) 
- Configured OSPF on R1, R2, and R3 for dynamic internal routing between all VLANs and departments
- Assigned router IDs and advertised connected subnets under router ospf
- Verified OSPF adjacencies and route propagation with show ip ospf neighbor and show ip route
- Configured EIGRP on R1 to exchange routes with the ASA firewall
- Verified mixed OSPF and EIGRP routes in the routing table
- Confirmed full connectivity between internal VLANs and outbound access toward the simulated Internet

- <img width="698" height="569" alt="image" src="https://github.com/user-attachments/assets/a8cea4ca-8d67-4a1b-9b37-010cdbc4152d" />

*Show IP Route Router1 (OSPF and EIGRP routes)

<img width="738" height="580" alt="image" src="https://github.com/user-attachments/assets/9b1055d0-b350-4737-b75e-fcb259daa06c" />

*Show IP Route Router2 (OSPF)*

<img width="694" height="610" alt="image" src="https://github.com/user-attachments/assets/88a75845-8ef7-450a-aa0b-16757e727258" />

*Show IP Router Router3*

 ### Phase Three: ACLs 
 To enforce department-based segmentation and limit unnecessary lateral communication between devices, access control lists (ACLs) were configured on the routers. These ACLs restrict traffic based on source/destination IP addresses and are applied to specific interfaces to control access to network resources such as servers and printers.
 #### Router2 
- Standard ACLs were configured to allow only specific departmental IP ranges to access the Sales/Marketing and Finance/Operations servers.
- Applied inbound on the interface connected to SW1 (MLSW1) hosting the servers.
- Ensures only authorized VLANs can communicate with the servers; all other access is denied.

  <img width="366" height="124" alt="image" src="https://github.com/user-attachments/assets/8ba96d7b-297e-4510-a8f7-3c230877e485" />

  *Standard IP access lists on R2 used to permit only specific department subnets to access Sales, Marketing, Finance, and Operations servers.*
  
 #### Router3
- Extended ACLs were configured to enforce strict printer access control.
- Each department’s PC can only communicate with its designated printer VLAN.
- ACLs were applied to the appropriate subinterfaces for each VLAN.
 
 <img width="492" height="393" alt="image" src="https://github.com/user-attachments/assets/ac9a8e19-f8dd-4333-91cc-a128b0dbafee" />

*Extended ACLs on R3 for Departmental Printer Segmentation*
