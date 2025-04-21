# BGP hijacking mitigations

BGP hijacking remains a critical threat to global internet routing. Below are practical mitigations, focusing on RPKI, 
BGPsec, real-time monitoring, and ISP cooperation.

| Technique	                | Effectiveness	               | Deployment difficulty     |
|---------------------------|------------------------------|---------------------------|
| RPKI (ROAs)	              | High (origin validation)	    | Medium (RIR coordination) |
| BGPsec	                   | Very High (path validation)	 | Low (limited support)     |
| Prefix Filtering (IRR)	   | Medium	                      | Easy (manual effort)      |
| BGP Monitoring (ARTEMIS)	 | High (auto-mitigation)	      | Medium (setup required)   |
| MANRS Compliance	         | High (industry cooperation)	 | Medium (policy change)    |


```{contents} Table of Contents
:depth: 3
```

## Deploy RPKI (Resource Public Key Infrastructure)

RPKI cryptographically validates that an AS (Autonomous System) is authorized to announce specific IP prefixes.
How to Implement RPKI

### For Network Operators (AS Owners)

Create ROAs (Route Origin Authorizations)

Use a RIR portal (ARIN, RIPE, APNIC) to authorize the prefixes.

Example (RIPE NCC):

```
Prefix: 192.0.2.0/24  
Max Length: 24  
ASN: AS12345 
```

Publish ROAs in the RPKI repository to ensure global validation.

### For ISPs & transit providers

Enable RPKI Validation on BGP Routers

Cisco IOS-XR:

```
router bgp 65000
 rpki server 203.0.113.1
  port 323
 address-family ipv4 unicast
  rpki origin-as validation enable
```

BIRD (Linux):

```
roa4 table r4;
protocol rpki {
  roa4 { table r4; };
  remote "203.0.113.1" port 323;
}
protocol bgp {
  ipv4 {
    import where roa_check(r4, net, bgp_path.last) = ROA_VALID;
  };
}
```

FRRouting (RPKI-Validator):

```
rpki
 rpki cache 203.0.113.1 323
!
address-family ipv4 unicast
 rpki validation
```

Limitations of RPKI:

* Does not prevent path hijacks (only origin validation).
* Adoption is still incomplete (~40% of routes are RPKI-validated as of 2024).

## Use BGPsec (Path Validation)

BGPsec extends RPKI to validate the entire AS path (not just origin).

* Currently limited support (Cisco, Juniper, BIRD).
* Requires all ASes in the path to support it (rare in practice).

Example (Cisco IOS-XR):

```
router bgp 65000
 bgpsec
  address-family ipv4 unicast
   bgpsec enable
```

## Implement Prefix Filtering & IRR (Internet Routing Registry)

* Filter invalid prefixes at peering edges.
* Use IRRDB (Internet Routing Registry Database) to automate filtering.

Example (BGP Peering Filter):

```
ip prefix-list PL_ACCEPTED_ROUTES seq 10 permit 192.0.2.0/24 le 24
route-map RM_PEER_IN permit 10
 match ip address prefix-list PL_ACCEPTED_ROUTES
!
router bgp 65000
 neighbor 203.0.113.2 route-map RM_PEER_IN in
```

## Monitor BGP in Real-Time

### BGP Monitoring Tools

| Tool	                 | Purpose                       |
|-----------------------|-------------------------------|
| BGPStream (RIS/Live)	 | Detects hijacks in real-time. |
| ARTEMIS (NVIDIA)	     | Automated hijack mitigation.  |
| Cloudflare Radar	     | Tracks routing anomalies.     |
| RIPE Stat	            | Historical BGP data.          |

### Detect Anomalies via RTT & TTL

* Round-Trip Time (RTT) changes → Possible hijack.
* Unexpected TTL jumps → Suspicious path alteration.
* Script Example (Python + scapy):

```python
from scapy.all import *
def detect_ttl_anomaly(pkt):
    if IP in pkt and pkt[IP].ttl != expected_ttl:
        print(f"TTL anomaly detected from {pkt[IP].src}")
sniff(filter="ip", prn=detect_ttl_anomaly)
```

## Collaborate with MANRS (Mutually Agreed Norms for Routing Security)

Join [MANRS](https://www.manrs.org/) to:

* Commit to anti-spoofing filters.
* Enable RPKI validation.
* Share real-time incident reports.

## Deploy automated mitigation

### ARTEMIS (Open-Source)

* Automatically withdraws hijacked routes.
* Integrates with RPKI/IRRDB.

### Cloudflare magic transit

Dynamically reroutes traffic during hijacks.

## ISP best practices

For Large ISPs:

* Mandate RPKI for all customers.
* Filter invalid prefixes at edges.

For Small ISPs:

* At least implement prefix-lists (manual IRR filtering).
* Patch BGP routers (e.g., Junos, IOS vulnerabilities).

