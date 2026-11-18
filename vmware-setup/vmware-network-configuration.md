# VMware Workstation Network Configuration

## 1. Project Overview

This document describes the virtual network infrastructure created in VMware Workstation 17 Pro for the SOC Home Lab.

The objective is to establish separate virtual network segments for the LAN, DMZ, server infrastructure, SOC monitoring, red-team activities and management network. pfSense acts as the central router and firewall between the lab networks.

## 2. Virtualisation Environment

| Component | Configuration |
|---|---|
| Hypervisor | VMware Workstation 17 Pro |
| Firewall | pfSense CE 2.6.0 |
| Host CPU | Intel Core i7-8700 @ 3.2 GHz |
| Host RAM | 64 GB |
| Host Storage | 1.75 TB SSD |
| Network architecture | Segmented virtual networks |

## 3. VMware Virtual Network Configuration

The following virtual networks have been created in VMware Virtual Network Editor.

| VMnet | Network Type | Subnet | VMware DHCP | Host Adapter |
|---|---|---|---|---|
| VMnet0 | Bridged | Physical network | Not used for lab | Not applicable |
| VMnet1 | Host-only | 192.168.237.0/24 | Enabled | Default host-only |
| VMnet2 | Host-only – LAN | 192.168.10.0/24 | Disabled | Enabled |
| VMnet3 | Host-only – DMZ | 192.168.20.0/24 | Disabled | Disabled |
| VMnet4 | Host-only – SERVER | 192.168.30.0/24 | Disabled | Disabled |
| VMnet5 | Host-only – SOC | 192.168.40.0/24 | Disabled | Disabled |
| VMnet6 | Host-only – RED TEAM | 192.168.50.0/24 | Disabled | Disabled |
| VMnet7 | Host-only – MANAGEMENT | 192.168.60.0/24 | Disabled | Enabled |
| VMnet8 | NAT | 192.168.163.0/24 | Enabled | NAT |

**Important:** VMnet0 and VMnet1 are not being used for the lab's virtual machines. VMnet8 is intended to provide the pfSense WAN connection through VMware NAT.

## 4. Host Virtual Adapter Configuration

The following host virtual adapter addresses are configured:

| Virtual Network | Host Adapter IP |
|---|---|
| VMnet2 – LAN | 192.168.10.1 |
| VMnet7 – MANAGEMENT | 192.168.60.1 |

Host virtual adapters are disabled on the DMZ, SERVER, SOC and RED TEAM networks to limit direct host connectivity to those segments.

## 5. Network Segmentation Design

| Network | Subnet | Intended Purpose |
|---|---|---|
| LAN | 192.168.10.0/24 | Windows client machines |
| DMZ | 192.168.20.0/24 | Public-facing or exposed lab services |
| SERVER | 192.168.30.0/24 | Windows Server and infrastructure services |
| SOC | 192.168.40.0/24 | Security monitoring and SIEM tools |
| RED TEAM | 192.168.50.0/24 | Kali Linux and controlled attack simulations |
| MANAGEMENT | 192.168.60.0/24 | Administrative access and management |

## 6. Design Considerations

- VMware DHCP is disabled on the custom lab networks.
- Static IP addressing is planned for the lab infrastructure.
- pfSense is intended to provide routing and firewall enforcement between the virtual networks.
- The lab networks are separated to support controlled security testing.
- VMware NAT is used for the intended pfSense WAN connection.
- Bridged networking is avoided for lab virtual machines to reduce exposure to the physical network.

**Security note:** Host-only networking alone does not prove that the lab is isolated. Firewall policies and connectivity tests must be completed before isolation can be claimed.

## 7. Verification and Evidence

The following screenshots should be stored in the repository:

`../screenshots/01-vmware-network-configuration/`

1. VMware Virtual Network Editor showing VMnet2–VMnet8.
2. LAN network configuration.
3. DMZ, SERVER, SOC and RED TEAM network configurations.
4. MANAGEMENT network configuration.
5. VMware NAT configuration for VMnet8.

## 8. Current Status

The VMware virtual networks have been created and configured according to the segmentation plan.

Further testing is required to verify routing, firewall enforcement and isolation between the different network segments.

## 9. Conclusion

The VMware network infrastructure provides the foundation for the SOC Home Lab. The segmented design allows different security environments to be developed and tested under the control of pfSense.
