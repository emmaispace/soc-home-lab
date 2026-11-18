# Connectivity and Network Isolation Testing

## 1. Introduction

This document records the connectivity and firewall isolation tests performed in the SOC home lab. The purpose of the tests was to verify that the Windows client machines could communicate with the pfSense LAN gateway, access the internet and resolve domain names while being prevented from accessing protected internal network segments.

The tests were conducted using two Windows 10 client machines connected to the LAN network managed by pfSense.

## 2. Objectives

The objectives of the testing were to:

- Verify connectivity between the Windows clients and the pfSense LAN gateway.
- Confirm that both Windows clients can access the internet.
- Verify DNS resolution using an external DNS server.
- Test whether the LAN clients can reach the protected internal network gateways.
- Confirm that pfSense blocks unauthorised traffic.
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

**Expected result:** The LAN clients should not be able to access these protected network gateways under the configured firewall policy.

**Observed result:** All five gateway tests failed with request timeouts. No ping replies were received.

This indicated that traffic from the LAN clients to the tested protected network gateways was being blocked.

## 5. pfSense Firewall Rule Configuration

The pfSense LAN firewall rules were examined to understand how the traffic restrictions were enforced.

The LAN rules included:

1. Anti-lockout rule.
2. A blocking rule named `BLOCK_LAN_TO_INTERNAL_NETWORKS`.
3. The default rule allowing LAN traffic to other destinations.
4. An IPv6 default allow rule.

The blocking rule was positioned before the general LAN allow rule. This ordering is important because pfSense evaluates firewall rules from the top down.

The blocking rule prevents traffic matching the internal network alias from proceeding to the general allow rule.

The WAN interface also showed no user-defined pass rules in the supplied screenshot. Incoming traffic on that interface is therefore blocked unless an applicable pass rule is added.

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

The test results demonstrate that pfSense is enforcing the intended LAN-to-internal-network restrictions.

Both Windows clients were able to communicate with their default gateway and access external network resources. DNS resolution also worked successfully.

However, attempts to access the tested protected network gateways were unsuccessful. The firewall logs confirmed that the ICMP traffic was blocked on the LAN interface.

This demonstrates the importance of firewall rule ordering and network segmentation in a security operations environment.

The tests also demonstrate how firewall logs can provide useful evidence when investigating denied network connections.

It is important to note that these tests verify access to the specified gateway addresses. They do not independently establish that every device or every possible traffic path between the internal network segments is isolated.

## 9. Conclusion

The connectivity and firewall isolation tests were successfully completed for both Windows clients.

The results confirmed that:

- Both clients can communicate with the pfSense LAN gateway.
- Both clients have internet connectivity.
- DNS resolution is functioning.
- Traffic directed towards the tested protected network gateways is blocked.
- pfSense records the blocked traffic in its firewall logs.

The results provide evidence that the pfSense firewall is enforcing the intended LAN access restrictions.

Further testing will be required to verify communication restrictions between all internal network segments and to confirm the complete isolation of the SOC lab.

## 10. Evidence and Screenshots

The screenshots supporting this testing exercise are stored in:

`screenshots/04-connectivity-and-isolation-tests/`

Evidence includes:

- Windows Client 01 LAN internet access test.
- Windows Client 01 LAN isolation test.
- Windows Client 01 DNS and internet connectivity test.
- Windows Client 02 LAN internet access test.
- Windows Client 02 LAN isolation test.
- Windows Client 02 DNS and internet connectivity test.
- pfSense LAN firewall rules.
- pfSense firewall logs showing blocked traffic from Client 01.
- pfSense firewall logs showing blocked traffic from Client 02.

## 11. Lessons Learned

This exercise provided practical experience in:

- Configuring and testing segmented virtual networks.
- Using pfSense as a network firewall and gateway.
- Understanding firewall rule processing and rule ordering.
- Using command-line tools such as `ping`, `ipconfig` and `nslookup` to troubleshoot connectivity.
- Analysing firewall logs to identify blocked traffic.
- Understanding how network segmentation contributes to a secure SOC environment.

The exercise establishes a foundation for subsequent SOC lab activities, including deploying monitoring systems, collecting network telemetry and investigating security events.
