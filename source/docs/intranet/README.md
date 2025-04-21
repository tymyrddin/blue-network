# Introduction

## What?

Intranet security mitigations are proactive measures to protect internal networks (LANs) from threats like lateral movement, data exfiltration, insider attacks, and compromised devices. Unlike the public internet, intranets often assume implicit trust, making them prime targets for attackers who breach perimeter defenses.

## Why?

* Prevent Lateral Movement: Attackers who breach one device (e.g., via phishing) can exploit weak internal controls to spread.
* Stop Insider Threats: Malicious or negligent employees can leak data or sabotage systems.
* Contain Compromised Devices: IoT devices, legacy systems, and unpatched endpoints are common attack vectors.
* Meet Compliance: Regulations (e.g., NIST, GDPR) require segmentation and access controls.

## How?

* [Secured NAT Router](add-nat.md)
* [LAN Segmentation](segmentation.md)
* [ARP Mitigations](arp.md)
* [DNS Mitigations](dns.md)
* [NFTables](nftables.md)
* [Snortbox as IDS](snort-box.md)
* [Beyond basic protections](beyond.md)