# Connectivity and Network Isolation Testing

## 1. Introduction

This document records the connectivity and firewall isolation tests performed in the SOC home lab. The purpose of the tests was to verify that the Windows client machines could communicate with the pfSense LAN gateway, access the internet and resolve domain names while being prevented from accessing the tested protected internal network gateways.

The tests were conducted using two Windows 10 client machines connected to the LAN network managed by pfSense.

## 2. Objectives

The objectives of the testing were to:

- Verify connectivity between the Windows clients and the pfSense LAN gateway.
- Confirm that both Windows clients can access the internet.
- Verify DNS resolution using an external DNS server.
- Test whether the LAN clients can reach the protected internal network gateways.
- Confirm that pfSense blocks traffic matching the configured firewall policy.
- Examine firewall logs to obtain evidence of blocked network traffic.

## 3. Lab Environment

The tests were conducted using the following network configuration.

| Device / Network | IP address | Subnet |
|---|---|---|
| pfSense LAN Gateway | 192.168.10.254 | 192.168.10.0/24 |
| Windows Client 01 | 192.168.10.10 | 192.168.10.0/24 |
| Windows Client 02 | 192.168.10.11 | 192.168.10.0/24 |
| DMZ Gateway | 192.168.20.1 | 192.168.20.0/24 |
| SERVER Gateway | 192.168.30.1 | 192.168.30.0/24 |
| SOC Gateway | 192.168.40.1 | 192.168.40.0/24 |
| RED TEAM Gateway | 192.168.50.1 | 192.168.50.0/24 |
| MANAGEMENT Gateway | 192.168.60.254 | 192.168.60.0/24 |
| External DNS Server | 1.1.1.1 | External |

**Firewall:** pfSense Community Edition 2.6.0

**Virtualisation platform:** VMware Workstation 17 Pro

## 4. Testing Methodology

The following commands were executed from the Windows client command prompt.

### 4.1 Verify IP configuration

Command:

```cmd
ipconfig /all
```

This command was used to examine the IP address, subnet mask, default gateway and DNS server configuration of the Windows clients.

The Windows clients were configured with static IP addresses:

- Client 01: 192.168.10.10
- Client 02: 192.168.10.11

Both clients used 192.168.10.254 as their default gateway.

### 4.2 Test LAN gateway connectivity

Command:

```cmd
ping 192.168.10.254
```

**Expected result:** Successful replies from the pfSense LAN gateway.

**Observed result:** Both Windows clients successfully communicated with the pfSense LAN gateway. The tests recorded four successful replies with no packet loss.

### 4.3 Test internet connectivity

Command:

```cmd
ping 1.1.1.1
```

**Expected result:** Successful replies from the external IP address.

**Observed result:** Both Windows clients successfully reached 1.1.1.1, with four packets received and no packet loss.

This confirmed that internet connectivity was working through the pfSense gateway.

### 4.4 Test DNS resolution

Command:

```cmd
nslookup google.com
```

**Expected result:** The DNS server should return IP addresses associated with google.com.

**Observed result:** DNS resolution was successful. The configured DNS server, 1.1.1.1, returned address records for google.com.

A further connectivity test was performed using:

```cmd
ping google.com
```

The domain name resolved to an IP address and the ping test received successful replies.

### 4.5 Test access to protected network gateways

The Windows clients were tested against the gateways of the protected network segments.

Commands:

```cmd
ping 192.168.20.1
ping 192.168.30.1
ping 192.168.40.1
ping 192.168.50.1
ping 192.168.60.254
```

**Expected result:** Traffic from the LAN clients to the protected network gateways should be blocked by the configured firewall policy.

**Observed result:** All five gateway tests failed with request timeouts. No ping replies were received.

The subsequent examination of pfSense firewall logs confirmed that the corresponding ICMP traffic from both Windows clients was blocked on the LAN interface.

## 5. pfSense Firewall Rule Configuration

The pfSense LAN firewall rules were examined to understand how the traffic restrictions were enforced.

The LAN rules included:

1. Anti-lockout rule – enabled.
2. `BLOCK_LAN_TO_INTERNAL_NETWORKS` – enabled.
3. Default allow LAN to any rule (IPv4) – enabled.
4. Default allow LAN IPv6 to any rule – disabled.

The blocking rule was positioned before the general LAN allow rule. This ordering is important because pfSense evaluates firewall rules from the top down.

The blocking rule uses the `LAB_INTERNAL_NETS` alias as its destination. It prevents matching traffic from proceeding to the general IPv4 allow rule.

The IPv6 default allow rule was disabled in accordance with the current IPv4-focused lab design.

## 6. Firewall Log Analysis

The pfSense firewall logs were examined under:

**Status → System Logs → Firewall → Dynamic View**

The logs recorded blocked ICMP traffic originating from both Windows clients.

The following traffic was identified:

| Source | Destination | Protocol | Interface | Action |
|---|---|---|---|---|
| 192.168.10.10 | 192.168.20.1 | ICMP | LAN | Block |
| 192.168.10.10 | 192.168.30.1 | ICMP | LAN | Block |
| 192.168.10.10 | 192.168.40.1 | ICMP | LAN | Block |
| 192.168.10.10 | 192.168.50.1 | ICMP | LAN | Block |
| 192.168.10.10 | 192.168.60.254 | ICMP | LAN | Block |
| 192.168.10.11 | 192.168.20.1 | ICMP | LAN | Block |
| 192.168.10.11 | 192.168.30.1 | ICMP | LAN | Block |
| 192.168.10.11 | 192.168.40.1 | ICMP | LAN | Block |
| 192.168.10.11 | 192.168.50.1 | ICMP | LAN | Block |
| 192.168.10.11 | 192.168.60.254 | ICMP | LAN | Block |

The firewall log entries displayed red block indicators, confirming that pfSense recorded these packets as blocked.

The logs also provided useful information, including source IP address, destination IP address, interface and protocol.

The log screenshots were captured using source IP filters for each Windows client, providing separate evidence of the blocked traffic.

## 7. Summary of Test Results

| Test | Client 01 | Client 02 | Result |
|---|---|---|---|
| LAN gateway connectivity | Successful | Successful | Passed |
| Internet connectivity | Successful | Successful | Passed |
| DNS resolution | Successful | Successful | Passed |
| DMZ gateway access | Blocked | Blocked | Passed |
| SERVER gateway access | Blocked | Blocked | Passed |
| SOC gateway access | Blocked | Blocked | Passed |
| RED TEAM gateway access | Blocked | Blocked | Passed |
| MANAGEMENT gateway access | Blocked | Blocked | Passed |

## 8. Security Analysis

The test results demonstrate that pfSense is enforcing the configured LAN-to-internal-network restrictions for the tested traffic.

Both Windows clients were able to communicate with their default gateway and access external network resources. DNS resolution also worked successfully.

However, attempts to access the five tested protected network gateways were unsuccessful. The firewall logs confirmed that the corresponding ICMP traffic was blocked on the LAN interface.

This demonstrates the importance of firewall rule ordering and network segmentation in a security operations environment.

The tests also demonstrate how firewall logs can provide useful evidence when investigating denied network connections. The source and destination IP addresses, interface and protocol information can assist an analyst in understanding the nature of a blocked connection.

It is important to note that these tests verify access to the specified gateway addresses using ICMP. They do not independently establish that every device or every possible TCP, UDP or other traffic path between internal network segments is isolated.

Further testing is therefore required to assess the effectiveness of the wider network segmentation policy.

## 9. Conclusion

The connectivity and firewall isolation tests were successfully completed for both Windows clients.

The results confirmed that:

- Both clients can communicate with the pfSense LAN gateway.
- Both clients have internet connectivity.
- DNS resolution is functioning.
- ICMP traffic directed towards the five tested protected network gateways is blocked.
- pfSense records the blocked traffic in its firewall logs.
- The configured LAN blocking rule is positioned above the general IPv4 allow rule.

The results provide evidence that the pfSense firewall is enforcing the intended LAN access restrictions for the traffic tested.

Further testing will be required to verify communication restrictions between all internal network segments and to assess the complete isolation of the SOC lab.

## 10. Evidence and Screenshots

The screenshots supporting this testing exercise are stored in:

`screenshots/04-connectivity-and-isolation-tests/`

Evidence includes:

- `Win-01-Test LAN internet access.png` – Client 01 internet connectivity.
- `Win-01-Test LAN isolation.png` – Client 01 protected gateway connectivity test.
- `Win-01-internet-dns-connectivity-test.png` – Client 01 DNS and internet connectivity.
- `Win-02-Test LAN internet access.png` – Client 02 internet connectivity.
- `Win-02-Test LAN isolation.png` – Client 02 protected gateway connectivity test.
- `Win-02-internet-dns-connectivity-test.png` – Client 02 DNS and internet connectivity.
- `pfSense-LAN-firewall-rules.png` – LAN firewall rule configuration.
- `pfSense-blocked-traffic-Win-01.png` – Firewall logs showing blocked traffic from Client 01.
- `pfSense-blocked-traffic-Win-02.png` – Firewall logs showing blocked traffic from Client 02.

## 11. Lessons Learned

This exercise provided practical experience in:

- Configuring and testing segmented virtual networks.
- Using pfSense as a network firewall and gateway.
- Understanding firewall rule processing and rule ordering.
- Using command-line tools such as `ping`, `ipconfig` and `nslookup` to troubleshoot connectivity.
- Analysing firewall logs to identify blocked traffic.
- Understanding how network segmentation contributes to a secure SOC environment.
- Using technical evidence to validate firewall behaviour and document security testing.

The exercise establishes a foundation for subsequent SOC lab activities, including deploying monitoring systems, collecting network telemetry and investigating security events.
