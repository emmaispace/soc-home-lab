
# VMware Workstation Environment – SOC Home Lab

## 1. Project Overview

This document describes the physical host and virtualisation environment supporting my SOC Home Lab.

The environment uses VMware Workstation 17 Pro to host virtual machines for learning enterprise networking, systems administration, cybersecurity monitoring and security analysis.

**Current status:** Virtual machine inventory confirmed. Network configuration and interface assignments are awaiting validation.

## 2. Physical Host Specifications

| Component | Specification |
|---|---|
| Processor | Intel Core i7-8700 @ 3.2 GHz |
| Memory | 64 GB RAM |
| Storage | 1.75 TB SSD |
| Hypervisor | VMware Workstation 17 Pro |
| Host OS | To be documented |

## 3. Virtual Machine Inventory

The following eight virtual machines are installed in the environment.

| Virtual Machine | Operating System | Intended Role |
|---|---|---|
| pfSense | pfSense | Firewall and routing |
| Windows Server 2022 | Windows Server 2022 | Server infrastructure and Active Directory |
| Windows 10 Client 01 | Windows 10 | Endpoint workstation |
| Windows 10 Client 02 | Windows 10 | Endpoint workstation |
| Kali Linux | Kali Linux | Controlled security testing |
| Security Onion | Security Onion | Network security monitoring |
| Wazuh | Linux-based deployment | Endpoint security monitoring |
| Splunk | Deployment OS to confirm | SIEM and log analysis |

Note: Installation does not necessarily mean that each machine is running or fully configured.

## 4. Virtual Network Architecture

The intended design separates the environment into different security zones.

| VMware Network | Network Name | Intended Subnet |
|---|---|---|
| VMnet2 | LAN | 192.168.10.0/24 |
| VMnet3 | DMZ | 192.168.20.0/24 |
| VMnet4 | SERVER | 192.168.30.0/24 |
| VMnet5 | SOC | 192.168.40.0/24 |
| VMnet6 | RED TEAM | 192.168.50.0/24 |
| VMnet7 | MANAGEMENT | 192.168.60.0/24 |
| VMnet8 | WAN / NAT | VMware NAT network |

These are intended network assignments. Actual VMware subnet, DHCP and adapter settings must be verified before implementation is marked complete.

## 5. Virtual Machine Roles

### 5.1 pfSense Firewall
- Central firewall and routing platform.
- Connects the virtual network security zones.
- Provides controlled communication between networks.
- Supports firewall rules, NAT and DHCP where configured.

### 5.2 Windows Server 2022
- Planned domain controller and infrastructure server.
- Active Directory and domain services.
- User and group administration.
- Windows security event logging.

### 5.3 Windows 10 Clients
- Simulate employee workstations.
- Support domain authentication exercises.
- Generate endpoint security events.
- Support controlled testing scenarios.

### 5.4 Kali Linux
- Controlled security testing environment.
- Network discovery and assessment exercises.
- Security testing against authorised lab systems.

### 5.5 Security Onion
- Network security monitoring.
- Network traffic analysis.
- Security alert investigation.

### 5.6 Wazuh
- Endpoint monitoring.
- Security event collection.
- Host-based security monitoring.

### 5.7 Splunk
- Security information and event management.
- Log searching and analysis.
- Security dashboard development.

## 6. Implementation Objectives

1. Verify VMware virtual network configuration.
2. Map pfSense network adapters to the correct virtual networks.
3. Configure WAN, LAN and additional firewall interfaces.
4. Establish network segmentation.
5. Configure Windows Server 2022 and Active Directory.
6. Connect Windows clients to the domain.
7. Deploy and configure Wazuh.
8. Configure Splunk log collection and monitoring.
9. Configure Security Onion for network monitoring.
10. Develop security investigation scenarios.

## 7. Evidence and Documentation

The following evidence will be maintained:

- VMware Virtual Network Editor screenshots.
- Virtual machine network adapter settings.
- pfSense interface assignments.
- IP addressing and subnet documentation.
- Firewall rules and connectivity tests.
- Server and endpoint configuration screenshots.
- Security monitoring dashboards.
- Troubleshooting records.

## 8. Learning Outcomes

The project aims to develop practical skills in:

- Virtualisation and infrastructure management.
- Network architecture and segmentation.
- Firewall administration.
- Windows Server and Active Directory.
- SIEM and security monitoring.
- Endpoint security.
- Network traffic analysis.
- Security investigation and technical documentation.

## Project Information

Author: Emmanuel Addo Larbi  
Location: Scotland, United Kingdom  
Platform: VMware Workstation 17 Pro  
Project: SOC Home Lab  
Career Focus: Entry-Level SOC Analyst

