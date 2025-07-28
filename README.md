# Enterprise Network Design with Dynamic Routing, VLAN Segmentation, and ASA Firewall Security

## Objective
The objective of this project was to design and implement a secure, enterprise-grade, multi-site network using Cisco routers, multilayer switches, and an ASA 5506-X firewall. The deployment incorporated dynamic routing protocols (OSPF and EIGRP), VLAN segmentation, DHCP configuration, SSH-enabled remote access, and extended ACLs to enforce interdepartmental isolation. The ASA firewall was configured to provide secure outbound internet connectivity using NAT, protocol inspection, and dynamic route exchange with internal routers via EIGRP. The overall design focused on layered security, traffic control, and operational scalability.

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
Successfully designed and deployed a secure, scalable enterprise network featuring VLAN segmentation, DHCP services, and dynamic routing using both OSPF and EIGRP. Enabled secure remote device access via SSH and implemented ACLs to enforce interdepartmental access control. The ASA 5506-X firewall was configured with NAT, protocol inspection, and dynamic EIGRP route exchange to provide secure outbound connectivity. Full functionality was verified through connectivity tests, routing table validation, and policy enforcement, ensuring a robust and secure network environment.

## Steps
### Phase One: Core Network Configuration 
<img width="1100" height="772" alt="image" src="https://github.com/user-attachments/assets/67162674-d464-425b-9f68-ffb748d5fd52" />

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

*Show IP Route Router1 (OSPF and EIGRP routes)*

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

*Extended ACLs on R3 for Departmental Endpoint Segmentation*

<img width="501" height="605" alt="image" src="https://github.com/user-attachments/assets/66d59bf9-9e05-43cb-ad3f-e48122469be9" />

<img width="600" height="583" alt="image" src="https://github.com/user-attachments/assets/bbe63d67-dc52-4f6e-a0f8-fd5dff5849fe" />

*Ping results confirming access to the assigned printer and ACL-enforced denial to other departmental devices*

 ### Phase Four: ASA Firewall Configuration 
 - Enabled ICMP and HTTP traffic inspection through the default class in the global policy
 - Configured dynamic NAT for inside subnet (10.0.0.0/8) to enable internet access

   <img width="375" height="95" alt="image" src="https://github.com/user-attachments/assets/8b32045c-c1ff-48c7-a92c-0d4fafeaf7b3" />


   <img width="533" height="86" alt="image" src="https://github.com/user-attachments/assets/25a56762-129d-4365-af09-4881b3cdea9a" />

*Verification of NAT configuration on ASA*

 - Allowed return traffic for ICMP and HTTP using stateful inspection, supporting external web browsing and internal ping tests

   <img width="414" height="203" alt="image" src="https://github.com/user-attachments/assets/4733d5fe-03ba-4c76-81a4-727ce1cbd9f5" />

*Verification of ASA inspection policy showing active inspection for DNS, FTP, HTTP, ICMP, and TFTP traffic*

 - Configured EIGRP on the ASA to enable dynamic route exchange with internal routers

   <img width="645" height="147" alt="image" src="https://github.com/user-attachments/assets/5523c975-20da-4b13-b3a9-3740b0d74d2d" />

*Verification of EIGRP neighbor relationships on the ASA firewall: Successful adjacency formed on Gig1/1 and Gig1/2 for process 100*

<img width="815" height="550" alt="image" src="https://github.com/user-attachments/assets/66ce9a39-2b34-4ff8-bc3d-ea8a2d9a6275" />

*Verification of EIGRP route propagation on the ASA: Dynamic routes successfully learned and installed from EIGRP neighbors on GigabitEthernet1*

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

*Show IP Route Router1 (OSPF and EIGRP routes)*

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

*Extended ACLs on R3 for Departmental Endpoint Segmentation*

<img width="501" height="605" alt="image" src="https://github.com/user-attachments/assets/66d59bf9-9e05-43cb-ad3f-e48122469be9" />

<img width="600" height="583" alt="image" src="https://github.com/user-attachments/assets/bbe63d67-dc52-4f6e-a0f8-fd5dff5849fe" />

*Ping results confirming access to the assigned printer and ACL-enforced denial to other departmental devices*

 ### Phase Four: ASA Firewall Configuration 
 - Enabled ICMP and HTTP traffic inspection through the default class in the global policy
 - Configured dynamic NAT for inside subnet (10.0.0.0/8) to enable internet access

   <img width="375" height="95" alt="image" src="https://github.com/user-attachments/assets/8b32045c-c1ff-48c7-a92c-0d4fafeaf7b3" />


   <img width="533" height="86" alt="image" src="https://github.com/user-attachments/assets/25a56762-129d-4365-af09-4881b3cdea9a" />

*Verification of NAT configuration on ASA*

 - Allowed return traffic for ICMP and HTTP using stateful inspection, supporting external web browsing and internal ping tests

   <img width="414" height="203" alt="image" src="https://github.com/user-attachments/assets/4733d5fe-03ba-4c76-81a4-727ce1cbd9f5" />

*Verification of ASA inspection policy showing active inspection for DNS, FTP, HTTP, ICMP, and TFTP traffic*

 - Configured EIGRP on the ASA to enable dynamic route exchange with internal routers

   <img width="645" height="147" alt="image" src="https://github.com/user-attachments/assets/5523c975-20da-4b13-b3a9-3740b0d74d2d" />

*Verification of EIGRP neighbor relationships on the ASA firewall: Successful adjacency formed on Gig1/1 and Gig1/2 for process 100*

<img width="815" height="550" alt="image" src="https://github.com/user-attachments/assets/66ce9a39-2b34-4ff8-bc3d-ea8a2d9a6275" />

*Verification of EIGRP route propagation on the ASA: Dynamic routes successfully learned and installed from EIGRP neighbors on GigabitEthernet1*


## Key Takeaways
- Implementing secure outbound access using ASA 5506-X with NAT and protocol inspection
- Dynamically routing traffic between multiple sites using OSPF and EIGRP
- Enforcing interdepartmental access control with standard and extended ACLs
- Enhancing internal security and traffic management through VLAN segmentation
- Automating IP address assignment with DHCP for efficient client provisioning
- Securing administrative access through SSH and CLI best practices

This project provided hands-on experience with enterprise-grade network design, dynamic routing, and firewall configuration—preparing for advanced roles in network security, infrastructure management, and cybersecurity operations.
