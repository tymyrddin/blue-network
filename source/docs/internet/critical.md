# Critical internet-wide mitigations

| Priority	 | Mitigation	             | Key Players                    |
|-----------|-------------------------|--------------------------------|
| 1	        | RPKI/BGPsec	            | ISPs, ICANN, Governments       |
| 2	        | QUIC/HTTP3	             | Google, Cloudflare, Mozilla    |
| 3	        | Post-Quantum TLS	       | NIST, Cloudflare, AWS          |
| 4	        | IoT Security Labels	    | EU, FCC, Device Manufacturers  |
| 5	        | DDoS Filtering (BCP38)	 | Tier-1 ISPs (e.g., Lumen, NTT) |


```{contents} Table of Contents
:depth: 3
```

## Universal adoption of RPKI & BGPsec

Problem: BGP hijacking still exploits weak origin validation.

Solution:

* Mandate RPKI (Route Origin Authorizations) for all ISPs.
* Deploy BGPsec for full path validation (beyond just origin checks).
* Policy push: Governments/ICANN should enforce compliance.

## QUIC/HTTP3 Encryption by default

Problem: ISPs/attackers sniff unencrypted DNS/HTTP traffic.

Solution:

* Deprecate plaintext protocols (HTTP, DNS-over-UDP).
* Enforce DoH (DNS-over-HTTPS) & DoT (DNS-over-TLS).
* Browser-level enforcement: Chrome/Firefox should block HTTP/1.1.

## Post-Quantum Cryptography (PQC) migration

Problem: Quantum computers will break RSA/ECC encryption.

Solution:

* Adopt NIST-standardized PQC algorithms (e.g., CRYSTALS-Kyber, Dilithium).
* Hybrid certificates (e.g., Cloudflare’s "Post-Quantum TLS").
* Protocol upgrades: TLS 1.3 + PQC key exchange.

## Mandatory DDoS mitigation for all networks

Problem: Volumetric attacks cripple unprotected networks.

Solution:

* Network-level filtering: BCP38 (anti-spoofing) compliance.

* Anycast scrubbing centers (e.g., Cloudflare, Akamai).

* ISP collaboration: Real-time attack blackholing.

## IoT security standards

Problem: Default credentials, no updates, botnet recruitment.

Solution:

* Legally mandated security labels (like EU’s Cyber Resilience Act).
* VLAN isolation + MAC filtering for IoT devices.
* Automated CVE patching (e.g., Microsoft’s Azure Sphere model).

## Decentralized identity (Beyond certificates)

Problem: Centralized CAs are single points of failure.

Solution:

* Web3 DID (Decentralized Identifiers) + VCs (Verifiable Credentials).
* Blockchain-anchered PKI (e.g., Ethereum ENS + SSL).

## AI-Powered threat detection

Problem: Human analysts can’t keep up with 0-day exploits.

Solution:

* Network-wide anomaly detection (e.g., Darktrace, CrowdStrike).
* Automated patch deployment (e.g., GitGuardian for secrets).

## Global Cyber warfare treaties

Problem: Nation-state attacks (e.g., SolarWinds, NotPetya).

Solution:

* Geneva Convention for cyberspace (ban critical infrastructure attacks).
* Attribution transparency: UN-backed forensic teams.

## Privacy-Enhancing technologies (PETs)

Problem: Mass surveillance & metadata leaks.

Solution:

* Oblivious DNS (ODoH) → Hide DNS queries from ISPs.
* SNI Encryption (e.g., ECH in TLS 1.3).
* Mandatory Tor/I2P support for sensitive services.

## Ethical hacktivism legalisation

Problem: Bug bounty programs are reactive, not proactive.

Solution:

* "Right to Repair" for cybersecurity: Allow reverse-engineering.
* Safe harbor laws for white-hat hackers.

## Why these mitigations matter

* Without RPKI: BGP hijacks will keep stealing cryptocurrency/traffic.
* Without PQC: Quantum computers will decrypt all historic TLS traffic.
* Without IoT Laws: Botnets will grow (Mirai 2.0).

