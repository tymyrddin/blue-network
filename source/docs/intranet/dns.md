# DNS mitigations

| Threat	                   | Mitigation                                 |
|---------------------------|--------------------------------------------|
| Cache Poisoning	          | DNSSEC, DNS query logging                  |
| Unauthorized DNS Servers	 | Firewall whitelisting, NAC                 |
| Malicious Domains	        | pDNS, Threat Intelligence Feeds            |
| DNS Exploits	             | IDS/IPS rules (Snort/Suricata)             |
| Rogue DNS Servers	        | Network scanning (nmap), Firewall blocking |

```{contents} Table of Contents
:depth: 3
```

## DNS Server audit trails & logging

Enable detailed DNS query logging on all resolvers (like BIND, Unbound, Windows DNS).

Example (BIND named.conf):

```
logging {
  channel query_log {
    file "/var/log/named/queries.log" versions 3 size 5m;
    severity dynamic;
    print-time yes;
  };
  category queries { query_log; };
};
```

* Monitor logs for anomalies (e.g., unexpected domains, unusual query volumes).
* Use tools like dnstop or dnsmon for real-time DNS traffic analysis.

## Whitelist valid DNS servers

Create a firewall whitelist of allowed DNS resolvers (like internal DNS + trusted public DNS like 1.1.1.1, 8.8.8.8).

Examples (Linux iptables):

```
iptables -A OUTPUT -p udp --dport 53 -j DROP  # Block all outgoing DNS 
iptables -A OUTPUT -p udp --dport 53 -d 1.1.1.1 -j ACCEPT  # Allow Cloudflare
iptables -A OUTPUT -p udp --dport 53 -d 8.8.8.8 -j ACCEPT  # Allow Google
```

Use NAC (Network Access Control) to enforce DNS server restrictions.

## DNSSEC (DNS security extensions)

Prevents cache poisoning by cryptographically signing DNS records.

Enable DNSSEC on your resolver:

BIND (named.conf):

```
options {
  dnssec-validation auto;
};
```

Unbound (unbound.conf):

```
server:
      module-config: "validator iterator"
```

Verify DNSSEC validation:

```
dig +dnssec example.com
```

## Block malicious DNS exploits

Firewall rules to block known malicious DNS patterns (like DNSChanger, OSX/MaMi).

Example (Suricata IDS rule):

```
alert dns any any -> any any (msg:"DNS Exploit Attempt"; content:"|6d 61 6d 69|"; nocase; sid:1000001; rev:1;)
```

Use threat intelligence feeds like AlienVault OTX, Cisco Talos, to block malicious domains.

## Monitor & detect suspicious DNS activity

Traffic Analyzer Scripts like Python + dpkt):

```python
import dpkt, socket
def analyze_dns(pcap):
    for ts, buf in pcap:
        eth = dpkt.ethernet.Ethernet(buf)
        if isinstance(eth.data, dpkt.ip.IP):
            ip = eth.data
            if isinstance(ip.data, dpkt.udp.UDP):
                udp = ip.data
                if udp.dport == 53:  # DNS queries
                    dns = dpkt.dns.DNS(udp.data)
                    print(f"Query: {dns.qd[0].name}")
```

Passive DNS Replication (pDNS)

* Use tools like dnstap or passivedns to log historical DNS data.
* Helps track newly registered malicious domains.

## Intrusion Detection Systems (IDS) for DNS

Snort/Suricata rules for DNS anomalies:

```
alert udp any any -> any 53 (msg:"Suspicious DNS Query"; content:"|00 01 00 00|"; depth:4; sid:1000002;)
```

Zeek (Bro) DNS monitoring:

```
zeek -i eth0 -C dns.log
```

Logs all DNS queries for later analysis.

## Block Unauthorized DNS Services

Detect rogue DNS servers with nmap:

```
nmap -sU -p 53 192.168.1.0/24
```

Block unauthorized DNS servers at the firewall level.

