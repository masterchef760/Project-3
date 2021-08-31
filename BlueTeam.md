# Blue Team: Summary of Operations
## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

## Network Topology

- The following machines were identified on the network:
1. Elk
  - Operating System: Linux
  - Purpose: Elasticsearch
  - IP Address: 192.168.1.100
2. Target 1
  - Operating System: Linux Debian 3.16.57-2
  - Purpose: Target for Red Team
  - IP Address: 192.168.1.110
3. Target 2
  - Operating System: Linux Debian 3.16.57-2
  - Purpose: Target for Red Team
  - IP Address: 192.168.1.115
4. Kali
  - Operating System: Kali Linux
  - Purpose: Attacker Machine
  - IP Address: 192.168.1.90

## Description of Targets
- The target of this attack was: Target 1 (192.168.1.110).
Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:


## Monitoring the Targets
- Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

## HTTP Request Size Monitor
  - This alert is implemented as follows:
  - Metric: http.request.bytes
  - Threshold: When sum of HTTP requests exceeds 3500 in one minute
  - Vulnerability Mitigated: Brute Force Attacks
  - Reliability: High reliability. Does not seem to generate very many (if any at all) false positives.

## Excessive HTTP Errors
  - This alert is implemented as follows:
  - Metric: http.response.status_code
  - Threshold: When the count of any one error exceeds 400 in 5 minutes
  - Vulnerability Mitigated: Brute Force Attacks
  - Reliability: High reliability. There should not be even close to 400 HTTP errors in 5 minutes.

## CPU Usage Monitor
- This alert is implemented as follows:
- Metric: system.process.cpu.total.pct
- Threshold: When the CPU reaches 50% usage
- Vulnerability Mitigated: Multiple attack types
- Reliability: Medium/low. The CPU reaching 50% usage does not necessarily indicate an attack is occurring. It is very possible for this to generate false positives.
