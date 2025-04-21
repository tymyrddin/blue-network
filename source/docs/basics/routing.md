# Routing audit

## Small Organisation Intranet Routing Audit

For organisations with OSPF/EIGRP, VLANs, and multiple sites

1. Identify Active Routing Protocols

```
show ip protocols        (Cisco)  
show running-config | section router (OSPF|EIGRP|RIP|BGP)  
netstat -rn              (Linux/Windows)
```

Check: Are unnecessary protocols (like RIP) running?

2. Verify Authentication

For OSPF:

```
show running-config | section ospf
# Look for:
#  area 0 authentication message-digest  
#  ip ospf message-digest-key 1 md5 YOUR_KEY
```

For EIGRP:

```
show running-config | section eigrp  
# Look for:  
#  authentication mode md5  
#  authentication key-chain MY_CHAIN  
```

Fix if missing: Enable MD5/SHA auth for all routing updates.

3. Check for Rogue Neighbors

```
show ip ospf neighbor      (OSPF)  
show ip eigrp neighbors    (EIGRP)
```

Action: Compare against a known list of legitimate routers.

4. Review Route Table for Anomalies

```
show ip route  
# Look for:  
# - Unexpected new routes  
# - Routes with suspicious next-hop IPs 
```

Use a tool for logging historical changes.

5. Test Traffic Paths

```
traceroute 10.0.1.100      (Linux/macOS)  
tracert 10.0.1.100         (Windows) 
```

Check: Does traffic take the expected path? If not, investigate.

6. Enable Logging & Alerts

Syslog example (Cisco):

```
logging host 192.168.1.100  
logging trap notifications
```

Tool: Forward logs to Graylog/Splunk for monitoring.

7. Apply Hardening Fixes

* Disable unused protocols (e.g., no router rip).
* Restrict routing updates with ACLs:

```
access-list 10 permit 192.168.1.0 0.0.0.255  
router ospf 1  
 distribute-list 10 in 
```

* Encrypt inter-site links with IPsec/GRE.

## Home/Lab Network Routing Audit

For home users with a router, VLANs, or small lab setups

1. Check Your Default Gateway

```
ip route show            (Linux)  
route print             (Windows)  
netstat -rn             (macOS) 
```

Check: Is your gateway IP correct? (No unexpected routes.)

2. Verify DHCP & Static Routes

On router (e.g., OpenWRT/ASUS):

```
cat /etc/config/network   # (OpenWRT)  
show running-config      # (Cisco home routers)
```

Fix: Remove unauthorized static routes.

3. Scan for Rogue Devices

```
nmap -sn 192.168.1.0/24 
```

Check: Are there unknown devices acting as routers?

4. Disable Risky Services

On home router:

* Turn off RIP/OSPF if not needed.
* Disable remote admin access (WAN-side management).

5. Enable Basic Firewall Rules

Example (OpenWRT):

```
# Block external routing protocols  
iptables -A INPUT -p 89 -j DROP    # OSPF  
iptables -A INPUT -p 88 -j DROP    # EIGRP
```

6. Test DNS & Traffic Redirection

Check for MiTM (On-Path):

```
dig example.com          # Look for unexpected DNS IPs  
traceroute 8.8.8.8       # Verify path to Google DNS 
```

7. Update Firmware & Reset Passwords

Router Admin Panel:

* Change default credentials (admin/password).
* Update firmware to patch routing exploits.

