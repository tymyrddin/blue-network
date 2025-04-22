# ARP mitigations

| Attack	        | Mitigation                     |
|----------------|--------------------------------|
| ARP Spoofing	  | Static ARP, DAI, Port Security |
| MITM Attacks	  | Encryption (VPN, HTTPS), VLANs |
| Rogue Devices	 | DHCP Snooping, MAC Filtering   |
| Detection	     | arpwatch, Wireshark, IDS       |

```{contents} Table of Contents
:depth: 3
```

## Static ARP entries (prevent spoofing)

Manually bind IPs to MACs on critical devices (servers, routers, gateways).

Linux (bash):

```
sudo arp -s <IP> <MAC>  # Permanent static entry
```

Windows (powershell)

```
netsh interface ipv4 add neighbors <InterfaceIndex> <IP> <MAC>
```

Cisco Router:

```
arp <IP> <MAC> arpa
```

Limitation: Hard to maintain in large networks.

## DHCP snooping + dynamic ARP inspection (DAI)

On Managed Switches (Cisco, HP, etc.)

Enable DHCP Snooping (trusts only authorized DHCP servers):

```
ip dhcp snooping
ip dhcp snooping vlan 10
interface Gig1/0/1
  ip dhcp snooping trust  # Allow DHCP server responses
```

Enable Dynamic ARP Inspection (DAI) (blocks fake ARP replies):

```
ip arp inspection vlan 10
interface Gig1/0/2
  ip arp inspection trust  # Trusted ports (e.g., routers)
```

## ARP monitoring tools

Linux: arpwatch (alerts on ARP changes):

```
sudo apt install arpwatch
sudo systemctl start arpwatch
```

    Windows: XArp (GUI-based ARP protection).

    Network-wide: RARP (Reverse ARP) for validation.

## Port security (MAC filtering)

Prevent unauthorized devices from connecting.

Example Cisco Switch:

```
interface Gig1/0/3
  switchport port-security
  switchport port-security maximum 1
  switchport port-security mac-address sticky
  switchport port-security violation restrict
```

Blocks unknown MACs and logs violations.

## VPN/Encryption (Mitigate MITM impact)

* SSL/TLS (HTTPS, SSH) prevents decryption even if ARP spoofed.
* IPSec VPNs encrypt all traffic between sites.

## Network segmentation (VLANs + Private VLANs)

* Isolate sensitive devices in [separate VLANs](segmentation.md).
* Private VLANs (PVLANs) block lateral ARP spoofing.

## ARP Spoofing detection tools

| Tool	                                                          | Function                                              |
|----------------------------------------------------------------|-------------------------------------------------------|
| ArpON (Linux)	                                                 | Blocks ARP attacks in real-time.                      |
| [Wireshark](https://nta.tymyrddin.dev/docs/wireshark/readme)   | Detects unusual ARP traffic (like duplicate replies). |
| [Snort](https://nta.tymyrddin.dev/docs/snort/readme)/Suricata	 | IDS rules for ARP anomalies.                          |
| ARPGuard (Windows)	                                            | Actively prevents spoofing.                           |