# LAN segmentation

Some routers offer the option to create virtual local area networks (VLANs) inside a larger private network. 

```{contents} Table of Contents
:depth: 3
```

## Benefits

* Guest Network Security – Visitors can’t access your main devices.
* IoT Protection – Smart devices can’t be hacked from infected PCs.
* Reduced Attack Surface – Malware can’t spread between VLANs.

## Plan Your VLAN Structure

Decide which devices should be isolated and assign them to separate VLANs:

* VLAN 10 (Main LAN) – Trusted devices (PCs, smartphones, servers).
* VLAN 20 (Guest Wi-Fi) – Visitors’ devices (prevents access to main LAN).
* VLAN 30 (IoT Devices) – Smart TVs, cameras, thermostats (restricted from other VLANs).

## Configuring VLANs on a Router/Switch

### Option A: Using a VLAN-Capable Router (e.g., pfSense, OpenWRT, Ubiquiti)

Create VLANs in the router’s admin panel:

* Assign VLAN IDs (e.g., 10, 20, 30).
* Set up DHCP scopes for each VLAN (e.g., 192.168.10.0/24, 192.168.20.0/24).

Configure Firewall Rules:

* Allow Guest VLAN (20) → Internet only (block access to other VLANs).
* Allow IoT VLAN (30) → Internet only (block access to Main LAN).

### Option B: Using a managed switch

If your router doesn’t support VLANs, use a managed switch (e.g., TP-Link, Cisco, Netgear):

Tag VLANs on switch ports:

* Port connected to router: Tagged (allows multiple VLANs).
* Port for Guest Wi-Fi AP: Untagged VLAN 20.
* Port for IoT devices: Untagged VLAN 30.

Set up a VLAN-aware Wi-Fi Access Point (e.g., OpenWRT, Ubiquiti):

* Create separate SSIDs for each VLAN (e.g., Home-WiFi (VLAN 10), Guest-WiFi (VLAN 20)).
* Enable WPA2/WPA3 with a strong password for guest networks.

## Apply Network Isolation (Critical Step!)

To prevent devices in one VLAN from accessing another:

* Enable "Client Isolation" on the Guest Wi-Fi (blocks device-to-device communication).
* Use firewall rules to:
    * Block VLAN 20 → VLAN 10 (Guests can’t access main LAN).
    * Block VLAN 30 → VLAN 10 (IoT can’t reach PCs/servers).
    * Allow VLAN 10 → VLAN 30 (Optional: If you need to control IoT devices from your main LAN).

Example (pfSense firewall rule for Guest VLAN):

| Rule	 | Source       | Destination	 | Port	 | Action                |
|-------|--------------|--------------|-------|-----------------------|
| 1	    | VLAN 20 Net	 | Any	         | *	    | Allow (Internet only) |
| 2	    | VLAN 20 Net	 | VLAN 10 Net  | 	*	   | Block                 |

## Test & verify isolation

* Connect a device to the Guest Wi-Fi and try pinging a device in the Main LAN (should fail).
* Check if IoT devices can still reach the internet (but not your PC).

5. Additional Security for IoT Devices

* Disable UPnP (prevents IoT devices from opening insecure ports).
* Use a separate SSID (e.g., Home-IoT with WPA2).
* Block IoT devices from initiating outbound connections (only allow responses).

