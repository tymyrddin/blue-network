# Defending from DDoS


```{contents} Table of Contents
:depth: 3
```

## On-Premises mitigations

### Network hardening

Enable BCP38 (Anti-Spoofing): Filter traffic with invalid source IPs at the edge.

Cisco example:

```
interface GigabitEthernet0/0
 ip verify unicast source reachable-via rx
```

### Rate limiting

Throttle suspicious traffic (e.g., SYN floods).

Linux (iptables):

```
iptables -A INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT
```

### Traffic scrubbing (DIY)

Deploy Open-Source Tools: FastNetMon (Real-time detection) + ExaBGP (BGP blackholing).

Suricata (L7 filtering):

```yaml
# suricata.yaml
threshold-file: /etc/suricata/threshold.config
```

##  ISP/Transit Provider responsibilities

| Provider Duty	     | Example                                                     |
|--------------------|-------------------------------------------------------------|
| Blackhole Routing	 | Drop attack traffic via RTBH (Remote Triggered Black Hole). |
| Flow Telemetry	    | Share NetFlow/sFlow data to identify attacks.               |
| Scaled Mitigation	 | Absorb large attacks (e.g., >100 Gbps).                     |

Does your ISP offer DDoS protection services? What is their process for attack mitigation?

## Cloud-Based protection (3rd Party)

For large-scale attacks, use cloud scrubbing centers:

| Service	                  | Best For	          | How It Works                                        |
|---------------------------|--------------------|-----------------------------------------------------|
| Cloudflare Magic Transit	 | Enterprises	       | Routes traffic through Cloudflare’s global network. |
| AWS Shield Advanced	      | AWS users	         | Automatic L3/L4 mitigation + WAF integration.       |
| Akamai Prolexic	          | High-risk targets	 | Always-on scrubbing.                                |

Setup Steps:

* Change DNS to point to the scrubbing service.
* BGP Announcement: Redirect traffic via cloud provider (e.g., Cloudflare Anycast).

## Application-layer defences

Stop HTTP floods, API abuse, and botnets.

### Web Application Firewall (WAF): 

Block OWASP Top 10 attacks (SQLi, XSS).

Cloudflare Rule Example:

```json
{
  "action": "challenge",
  "expression": "(http.request.uri.path contains '/wp-admin')"
}
```

### Rate Limiting

Nginx:

```
limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
```

## Emergency response plan

Detection: Use FastNetMon or ISP alerts.

Mitigation:

* Small Attacks: On-prem rate limiting.
* Large Attacks: Redirect to cloud scrubbing (update BGP).

Post-Attack: Analyze logs to block future attacks.
        
