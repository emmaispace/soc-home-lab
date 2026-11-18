# SOC Home Lab – Building a Virtual Security Operations Centre

## 1. Project Overview

This project documents the design, implementation and development of a virtual Security Operations Centre (SOC) home lab using VMware Workstation 17 Pro.

The lab provides a controlled virtual enterprise environment for developing practical cybersecurity skills in network security, firewall administration, security monitoring, log management, threat detection and incident investigation.

The infrastructure is built around pfSense as the central firewall and network gateway, with segmented virtual networks designed to support Windows infrastructure, security monitoring platforms and controlled security testing.

The project follows a practical, evidence-based approach. Each implementation stage is documented through configuration records, screenshots, connectivity tests, firewall logs, troubleshooting activities and lessons learned.

**Project Status:** Initial virtual network infrastructure configured. LAN connectivity, internet access, DNS resolution and selected firewall isolation tests successfully verified. Further implementation and testing are ongoing.

---

## 2. Project Objectives

The main objectives of this project are to:

- Build a virtual enterprise network environment using VMware Workstation.
- Configure pfSense as the central firewall and network gateway.
- Implement network segmentation and access control policies.
- Deploy Windows Server and Active Directory infrastructure.
- Configure endpoint security monitoring and log collection.
- Deploy and explore SIEM technologies.
- Implement network security monitoring and intrusion detection.
- Develop security dashboards and investigate security events.
- Simulate controlled attacks and analyse their associated security events.
- Develop practical SOC Analyst skills through hands-on exercises.
- Maintain technical documentation and evidence of each implementation stage.

---

## 3. Lab Environment and Technologies

### 3.1 Virtualisation Environment

| Component | Configuration |
|---|---|
| Hypervisor | VMware Workstation 17 Pro |
| Host Processor | Intel Core i7-8700 @ 3.2 GHz |
| Host Memory | 64 GB RAM |
| Storage | 1.75 TB SSD |
| Firewall Platform | pfSense CE 2.6.0 |
| Network Architecture | Segmented virtual networks |

### 3.2 Technologies and Tools

| Technology | Purpose |
|---|---|
| VMware Workstation | Virtual machine hosting and network virtualisation |
| pfSense | Firewall, routing and network access control |
| Windows Server 2022 | Server infrastructure |
| Active Directory | Identity and domain management |
| Windows 10 | Endpoint testing and monitoring |
| Wazuh | Security monitoring and endpoint detection |
| Splunk | SIEM, log analysis and security dashboards |
| Security Onion | Network security monitoring |
| Zeek | Network traffic analysis |
| Suricata | Network intrusion detection |
| Kali Linux | Controlled security testing |

---

## 4. Network Architecture and Segmentation

The lab uses a segmented network design to separate client systems, servers, monitoring infrastructure, management access and controlled security testing.

pfSense provides the routing and firewall functionality between the virtual networks.

### 4.1 Network Addressing Plan

| Network | VMware Network | Subnet | pfSense Gateway | Purpose |
|---|---|---|---|---|
| WAN | VMnet8 | 192.168.163.0/24 | DHCP | External connectivity through VMware NAT |
| LAN | VMnet2 | 192.168.10.0/24 | 192.168.10.254 | Windows client machines |
| DMZ | VMnet3 | 192.168.20.0/24 | 192.168.20.1 | Public-facing lab services |
| SERVER | VMnet4 | 192.168.30.0/24 | 192.168.30.1 | Server infrastructure |
| SOC | VMnet5 | 192.168.40.0/24 | 192.168.40.1 | Security monitoring infrastructure |
| RED TEAM | VMnet6 | 192.168.50.0/24 | 192.168.50.1 | Controlled attack simulations |
| MANAGEMENT | VMnet7 | 192.168.60.0/24 | 192.168.60.254 | Administrative access |

### 4.2 Windows Client Addressing

| Device | IPv4 Address | Default Gateway |
|---|---|---|
| Windows Client 01 | 192.168.10.10 | 192.168.10.254 |
| Windows Client 02 | 192.168.10.11 | 192.168.10.254 |

Both Windows clients use static IPv4 addressing.

### 4.3 Network Security Design

The network design uses separate virtual network segments to control communication between different parts of the lab.

The intended security objectives include:

- Restricting unnecessary communication between internal network segments.
- Allowing authorised LAN clients to access the internet through pfSense.
- Preventing unrestricted access from the LAN to protected internal networks.
- Restricting administrative access to authorised management systems.
- Providing a separate environment for controlled red-team activities.
- Supporting future security monitoring and traffic analysis.

---

## 5. Implementation Progress

### Phase 1 – Virtualisation Environment

**Status: Initial setup completed**

Activities completed:

- VMware Workstation 17 Pro prepared.
- Virtual network infrastructure created.
- Custom virtual network segments configured.
- Separate LAN, DMZ, SERVER, SOC, RED TEAM and MANAGEMENT networks established.
- VMware DHCP disabled on the custom lab networks where static addressing is intended.

### Phase 2 – Network Infrastructure and Firewall

**Status: Initial configuration completed; further testing ongoing**

Activities completed:

- pfSense CE 2.6.0 installed.
- Seven network interfaces assigned.
- Interfaces renamed WAN, LAN, DMZ, SERVER, SOC, RED_TEAM and MANAGEMENT.
- Static IPv4 addresses configured for the internal interfaces.
- WAN configured to obtain its address through VMware NAT.
- pfSense web administration interface accessed successfully.
- Initial firewall rules examined.
- Internal network alias created: `LAB_INTERNAL_NETS`.
- LAN firewall blocking rule created: `BLOCK_LAN_TO_INTERNAL_NETWORKS`.
- LAN IPv6 default allow rule disabled for the current IPv4-focused lab design.

### Phase 3 – Windows Client Connectivity and Testing

**Status: Initial connectivity and selected isolation tests verified**

Two Windows 10 clients were configured on the LAN network.

Activities completed:

- Windows Client 01 configured with `192.168.10.10`.
- Windows Client 02 configured with `192.168.10.11`.
- Both clients configured with `192.168.10.254` as their default gateway.
- Connectivity to the pfSense LAN gateway verified.
- Internet connectivity through pfSense verified.
- DNS resolution using external DNS verified.
- Attempts to reach the five protected network gateways tested.
- pfSense firewall logs examined to confirm blocked ICMP traffic from both Windows clients.

The results demonstrate that the clients retain internet connectivity while traffic to the tested protected network gateways is blocked by the configured LAN firewall policy.

Further testing is required to verify TCP and UDP restrictions and communication between all internal network segments.

### Phase 4 – Windows Infrastructure and Active Directory

**Status: Planned**

Planned activities:

- Configure Windows Server 2022.
- Deploy Active Directory Domain Services.
- Configure DNS.
- Create domain users and groups.
- Join Windows clients to the domain.
- Implement Group Policy security configurations.

### Phase 5 – Endpoint Security Monitoring

**Status: Planned**

Planned activities:

- Deploy Wazuh.
- Install and configure Windows agents.
- Collect Windows security events.
- Monitor authentication activity.
- Investigate endpoint security alerts.

### Phase 6 – SIEM Implementation

**Status: Planned**

Planned activities:

- Deploy Splunk.
- Configure log ingestion.
- Forward Windows event logs.
- Develop security monitoring dashboards.
- Investigate failed logons and suspicious activities.

### Phase 7 – Network Security Monitoring

**Status: Planned**

Planned activities:

- Deploy Security Onion.
- Configure Zeek.
- Configure Suricata.
- Monitor network traffic.
- Analyse network security alerts.
- Investigate suspicious network activities.

### Phase 8 – Security Investigation and Detection Engineering

**Status: Planned**

Planned activities:

- Develop controlled attack scenarios.
- Investigate authentication events.
- Analyse suspicious activities.
- Develop detection rules.
- Document incident investigation reports.
- Explore threat detection and response techniques.

---

## 6. Project Documentation and Evidence

The project uses GitHub to maintain technical documentation and evidence of implementation.

The repository is organised as follows:

```text
soc-home-lab/
│
├── 00-lab-architecture/
│
├── active-directory/
│
├── investigation-reports/
│
├── screenshots/
│
├── security-onion/
│
├── splunk/
│
├── vmware-setup/
│   ├── connectivity-and-isolation-testing.md
│   └── vmware-network-configuration.md
│
├── wazuh/
│
├── windows-server/
│
├── zeek-suricata/
│
└── README.md
```

Each implementation stage is supported by relevant documentation, including:

- Project objectives and requirements.
- Installation and configuration procedures.
- Network addressing and architecture.
- Commands and configuration details.
- Screenshots and technical evidence.
- Connectivity and security testing.
- Firewall log analysis.
- Troubleshooting and lessons learned.

---

## 7. Current Project Achievements

The following achievements have been verified during the initial lab implementation:

| Activity | Status |
|---|---|
| VMware virtual network configuration | Completed |
| pfSense installation | Completed |
| pfSense interface assignment | Completed |
| Internal network addressing | Configured |
| Windows Client 01 configuration | Verified |
| Windows Client 02 configuration | Verified |
| LAN gateway connectivity | Verified |
| Internet connectivity | Verified |
| External DNS resolution | Verified |
| LAN firewall isolation rule | Configured |
| Blocked ICMP traffic verification | Verified through pfSense logs |
| Connectivity and isolation documentation | Prepared |
| Complete inter-network isolation testing | Ongoing |
| Network architecture diagram | Pending |
| VMware snapshot management | Pending confirmation |
| Active Directory deployment | Planned |
| Wazuh and Splunk deployment | Planned |
| Security Onion deployment | Planned |

---

## 8. Skills Being Developed

This project provides practical experience in:

- Network Security and Segmentation
- Firewall Administration
- VMware Virtualisation
- Windows Server Administration
- Active Directory and DNS
- SIEM Operations
- Security Event Monitoring
- Log Analysis
- Network Traffic Analysis
- Intrusion Detection
- Threat Detection
- Incident Investigation
- Technical Documentation

---

## 9. Project Disclaimer

This is an educational cybersecurity home-lab project developed in a controlled virtual environment for learning and professional development.

All security testing and attack simulations are intended to take place within the authorised lab environment.

The project represents practical learning activities and does not claim production SOC experience or professional security operations employment.

---

**Author:** Emmanuel Addo Larbi  
**Career Focus:** SOC Analyst / Cybersecurity Analyst  
**Location:** Scotland, United Kingdom  

**GitHub:** [emmaispace/soc-home-lab](https://github.com/emmaispace/soc-home-lab)
