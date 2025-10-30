
# Proof of Concept: DDoS Simulation and Analysis
Author: Vignesh G
Organization: Digit Defence (POC Internship)
Date: January 2025

This POC simulates a TCP-based DDoS (SYN flood) in a controlled lab to observe attack patterns, validate
detection by Snort IDS, and capture traffic with Wireshark. The test generated approximately 50k + 
packets/sec toward the target.
Findings show clear SYN-flood characteristics, increased latency, and Snort
alerts. Recommendations focus on network-level hardening and IDS tuning.


## Objective and scope
Objective: Simulate a high rate TCP SYN flood to evaluate detection capability and observe network behavior
under load.
Scope: Single lab environment with attacker and target VMs. Tools used: Hping3, Wireshark, Snort IDS. Tests
restricted to lab VMs only.


## Test environment
● Attacker: Kali Linux (virtual machine)
● Target: Linux Mint(virtual machine)
● Network mode: Virtual network (bridged or host-only per lab setup)
● Monitoring: Wireshark for packet capture; Snort for intrusion detection and alerting
● Test window: Jan–Feb 2025 (performed during internship POC)


## Pre-test configuration
1. Ensure attacker and target are isolated from any external networks.
Start Snort on the target for real-time alerting:
sudo snort -c /etc/snort/snort.conf -i <Interface name> -A console
Prepare Wireshark capture on the target or a mirror interface and start capture before generating traffic.


## Attack methodology
Attack used: TCP SYN flood using Hping3.
Command executed on attacker (example):
Flood with SYN packets using random source IPs
sudo hping3 -S -p 80 --flood --rand-source <TARGET_IP>
Approximate packet rate observed: ~5,000 packets/sec (measured during tests)
Notes:
● -S sets the SYN flag.
● --flood sends packets as fast as possible.
● --rand-source randomizes source IPs to emulate spoofing.


## Data collection / Evidence
### Wireshark observations
● Filter used: tcp.flags.syn == 1 && tcp.flags.ack == 0
● Observed pattern: large stream of SYN packets to destination IP and port 80 with many unique source
IPs (when using --rand-source).
● Metrics captured:
○ SYN packet rate peaked near the test rate (~5k pkts/sec).
○ Very low proportion of SYN-ACK/ACK completions (indicating flood rather than legitimate
connection attempts).
### Snort alerts
● Snort produced multiple alerts corresponding to TCP flood signatures and anomalous SYN rate spikes.
● Alerts included timestamps, source IP (often spoofed), destination, and signature name (console output
and/or alert log).
Target VM behavior
● Increased HTTP response time during peak traffic.
● Network latency rose; server CPU and network interface reported higher utilization.
● No permanent system crash observed in the lab; the test validated detection, not complete service
destruction.

## Recommendations (implementation-ready)
Network-level mitigations
Enable SYN cookies on Linux:
sudo sysctl -w net.ipv4.tcp_syncookies=1
#To persist, add to /etc/sysctl.conf:
#net.ipv4.tcp_syncookies=1
Rate-limit SYN packets with iptables:
allow 10 syn packets per second with burst of 20
sudo iptables -A INPUT -p tcp --syn -m limit --limit 10/s --limit-burst 20 -j ACCEPT
sudo iptables -A INPUT -p tcp --syn -j DROP
## Firewall / upstream
● Deploy upstream filtering (ISP or cloud provider) or use a CDN/WAF that supports DDoS mitigation if
service is public-facing.
● Use blackhole routing temporarily for extreme floods when coordinated with upstream provider.
Limitations and assumptions
● Test performed in an isolated lab environment. Results may differ on production networks with different
capacity, middleboxes, and upstream providers.
● Attack traffic was simulated; source IP spoofing behavior depends on your lab network configuration
and hypervisor networking mode.
● This POC focused on SYN flood characteristics. Other DDoS vectors (UDP floods, application-layer
floods, amplification attacks) were not tested here.

## Conclusion
The POC validated that a high-rate SYN flood produces clear, detectable signatures in packet captures and
that Snort can detect such behavior. Basic kernel and firewall mitigations (SYN cookies, rate-limiting) reduce
impact. For public services, upstream and cloud mitigations are recommended along with IDS tuning and
monitoring.
DEMO:
