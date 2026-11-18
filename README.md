# SOC Home Lab – Building a Virtual Security Operations Centre

## Project Overview

This project documents the development of a virtual Security Operations Centre (SOC) home lab using VMware Workstation.

The objective is to develop practical knowledge and skills in cybersecurity, network defence, security monitoring, log management, threat detection and incident investigation.

The lab brings together virtual network infrastructure, Windows systems, security monitoring platforms and network security tools to simulate selected elements of an enterprise IT and security environment.

**Project Status:** Initial environment setup completed; configuration and practical testing are ongoing.

## Project Objectives

- Build a virtual enterprise network environment.
- Configure firewall and network security controls.
- Deploy Windows Server and Active Directory.
- Develop endpoint monitoring capabilities.
- Explore SIEM technologies and security event analysis.
- Practise network traffic monitoring and intrusion detection.
- Develop security dashboards and document investigation exercises.
- Maintain technical documentation of configurations, challenges and lessons learned.

## Technologies and Tools

| Technology | Purpose |
|---|---|
| VMware Workstation | Virtualisation |
| pfSense | Firewall and network routing |
| Windows Server | Server administration |
| Active Directory | Identity and domain management |
| Windows Clients | Endpoint environment |
| Splunk | SIEM and log analysis |
| Wazuh | Security monitoring |
| Security Onion | Network security monitoring |
| Zeek | Network traffic analysis |
| Suricata | Intrusion detection |

## Planned Lab Architecture

The project is designed around the following network environments:

- LAN – Internal client network.
- DMZ – Isolated network for public-facing services.
- Server Network – Infrastructure and server systems.
- SOC Network – Security monitoring and analysis.
- Red Team Network – Controlled security testing environment.
  

Detailed IP addressing, network diagrams and firewall configurations will be documented as the environment is configured.

## Implementation Roadmap

### Phase 1 – Virtualisation Environment
- Install and configure VMware Workstation.
- Prepare the required virtual machines.
- Review virtual machine resources and networking.

### Phase 2 – Network Infrastructure
- Configure pfSense.
- Establish network interfaces and segmentation.
- Configure firewall rules and routing.
- Test network connectivity.

### Phase 3 – Windows Infrastructure
- Configure Windows Server.
- Deploy Active Directory.
- Create domain users and groups.
- Join Windows clients to the domain.

### Phase 4 – Security Monitoring
- Deploy Wazuh.
- Configure endpoint monitoring.
- Explore Windows security event collection.

### Phase 5 – SIEM Implementation
- Install and configure Splunk.
- Explore log ingestion and searching.
- Develop security monitoring dashboards.

### Phase 6 – Network Security Monitoring
- Deploy Security Onion.
- Explore Zeek and Suricata.
- Analyse network traffic and security alerts.

### Phase 7 – Security Investigation
- Develop controlled security testing scenarios.
- Examine authentication events.
- Investigate suspicious activities.
- Document findings and lessons learned.

## Project Documentation

Each implementation stage will include:

- Objectives and requirements.
- Installation and configuration steps.
- Network diagrams where relevant.
- Screenshots and supporting evidence.
- Testing procedures and results.
- Troubleshooting notes.
- Lessons learned.

## Skills Being Developed

- Network Security
- Windows Server Administration
- Active Directory
- SIEM Operations
- Security Event Monitoring
- Log Analysis
- Network Traffic Analysis
- Intrusion Detection
- Incident Investigation
- Technical Documentation

## Disclaimer

This is an educational home-lab project developed in a controlled virtual environment for learning and professional development. It does not represent production security operations or professional SOC employment experience.

---

**Author:** Emmanuel Addo Larbi  
**Location:** Scotland, United Kingdom  
**Career Focus:** Entry-Level SOC Analyst / Cybersecurity Analyst

GitHub: https://github.com/emmaispace
