# Beyond basic protections

| Category	          | Mitigations                                       |
|--------------------|---------------------------------------------------|
| Segmentation	      | VLANs, Zero Trust, SDN                            |
| Monitoring	        | NetFlow, TLS inspection, SIEM                     |
| Endpoint Security	 | NAC, Host firewalls, EDR                          |
| Threat Hunting	    | Honeypots, EDR, deception tech                    |
| Physical Security	 | Disabled ports, locked racks, supply chain checks |
| Wireless	          | WPA3-Enterprise, rogue AP detection               |


```{contents} Table of Contents
:depth: 3
```

## Network Segmentation & Zero Trust

### Micro-Segmentation

Isolate critical systems (e.g., Whatever, HR) using VLANs or private VLANs (PVLANs).

Example (Cisco):

```
vlan 10
  name Whatever
private-vlan primary
private-vlan association 20
```

Use Zero Trust principles:

* Never trust, always verify (even inside the LAN).
* Enforce device authentication (802.1X, certificates).

### Software-Defined Networking (SDN)

Centralized traffic control (e.g., OpenFlow) to detect lateral movement.

Tools: OpenDaylight, VMware NSX.

## Advanced traffic monitoring

### NetFlow/sFlow Analysis

Detect anomalies (e.g., data exfiltration, lateral movement).

Tools:

* Elasticsearch + Packetbeat (log analysis).
* Darktrace (AI-based anomaly detection).

### Encrypted Traffic Inspection

Decrypt & inspect TLS (for malware C2, data leaks).

Tools: Palo Alto SSL Decryption, Squid + SSL bump.

Caution: Privacy/legal implications.

## Endpoint & server hardening

### Network Access Control (NAC)

Allow only compliant devices (e.g., updated OS, antivirus).

Tools: Cisco ISE, Forescout, PacketFence.

802.1X/EAP-TLS for wired/wireless auth.

### Host-Based Firewalls

Enforce least privilege (block unnecessary inbound/outbound).

Windows: Set-NetFirewallProfile -Enabled True

Linux: ufw default deny incoming

### Disable Legacy Protocols

Block SMBv1, LLMNR, NetBIOS (common attack vectors):
    
```
Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
```

## Threat hunting & deception

### Honeypots

Deploy fake servers to detect intruders.

Tools: CanaryTokens, T-Pot.

Example: Fake SMB share with logging.

### Endpoint Detection & Response (EDR)

Use [EDR](https://edr.tymyrddin.dev/) to monitor processes, memory, network for malware.

Tools: CrowdStrike, SentinelOne, Wazuh (open-source).

## Physical & supply chain security

### Secure Network Hardware

Disable unused switch ports:

```
interface range Gig1/0/1-24
  shutdown
```

Lock server racks (prevent tampering).

### Firmware integrity checks

Verify router/switch firmware (no backdoors):

```
sha256sum firmware.bin | grep EXPECTED_HASH
```

## Logging & incident response

### Centralized SIEM

Aggregate logs (firewall, DNS, endpoints).

* Tools: Splunk, Graylog, ELK Stack.
* Alert on: Brute-force, unusual logins.

### Automated Response

Block malicious IPs automatically:

```
# Fail2Ban rule for SSH
[sshd]
enabled = true
banaction = iptables-multiport
```

## Wireless Security (If applicable)

### WPA3-Enterprise

Require certificates (not passwords).

Example (FreeRADIUS):

```
Auth-Type = EAP
```

### Rogue AP detection

Tools: Kismet, AirDefense.

## And more

* Audit your network with `nmap -sV 192.168.1.0/24`.
* Deploy a [SIEM](https://siem.tymyrddin.dev/) (start with Wazuh for free).
* Test defenses with Caldera (MITRE ATT&CK simulations).

