
# VMware Workstation Environment

## 1. Overview

The SOC home lab is hosted on a physical computer running VMware Workstation 17 Pro.

The virtualisation platform provides an environment for developing and testing enterprise networking, Windows infrastructure and cybersecurity monitoring technologies.

## 2. Physical Host Specifications

| Component | Specification |
|---|---|
| Processor | Intel Core i7-8700 |
| CPU Speed | 3.2 GHz |
| RAM | 64 GB |
| Storage | 1.75 TB SSD |
| Hypervisor | VMware Workstation 17 Pro |

## 3. Virtualisation Environment

VMware Workstation is used to create and manage virtual machines and virtual networks.

The environment is designed to separate internal users, servers, monitoring infrastructure and security testing systems.

## 4. Virtual Machine Inventory

| Virtual Machine | Operating System | Purpose |
|---|---|---|
| pfSense | pfSense | Firewall and routing |
| Kali Linux | Kali Linux | Security testing |
| DC01 | Windows Server 2019 | Active Directory |
| Windows Client 01 | Windows 10 | Endpoint |
| Windows Client 02 | Windows 10 | Endpoint |
| Security Onion | Security Onion | Network monitoring |
| Wazuh | Linux deployment | Security monitoring |
| Splunk | Linux/Windows deployment | SIEM |

*Inventory entries must be verified against the current VMware installation before being treated as active systems.*

## 5. Planned Virtual Network Architecture

| VMware Network | Network Role | Subnet |
|---|---|---|
| VMnet2 | LAN | 192.168.10.0/24 |
| VMnet3 | DMZ | 192.168.20.0/24 |
| VMnet4 | Server | 192.168.30.0/24 |
| VMnet5 | SOC | 192.168.40.0/24 |
| VMnet6 | Red Team | 192.168.50.0/24 |
| VMnet7 | Management | 192.168.60.0/24 |
| VMnet8 | NAT / WAN | VMware NAT network |

## 6. Project Objectives

- Develop an isolated virtual enterprise network.
- Practise network segmentation and firewall configuration.
- Deploy and manage Windows infrastructure.
- Explore SIEM and endpoint monitoring technologies.
- Develop network security monitoring capabilities.
- Document technical configurations and troubleshooting activities.

## 7. Evidence and Documentation

Screenshots, virtual machine configurations, network diagrams and implementation notes will be added as each stage is completed.

## 8. Learning Outcomes

This environment is intended to support practical learning in:

- Virtualisation
- Network infrastructure
- Windows Server administration
- Active Directory
- Network security
- SIEM
- Security monitoring
- Incident investigation

---
**Project:** SOC Home Lab  
**Author:** Emmanuel Addo Larbi  
**Platform:** VMware Workstation 17 Pro

