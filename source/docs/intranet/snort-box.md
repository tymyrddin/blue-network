# Snortbox as IDS

```{contents} Table of Contents
:depth: 3
```

To implement a Snort IDS (Intrusion Detection System) server that monitors traffic intended for existing central 
servers without having its own IP address, it will need to be configured in promiscuous mode and to apply specific 
rules to filter traffic. 

## Promiscuous mode setup

    The Snort box must operate in promiscuous mode to capture all network traffic, even packets not addressed to it.

Ensure the network interface card (NIC) is configured to accept all traffic (Replace <interface> with your NIC, 
e.g., eth0 or ens33.):

```
sudo ifconfig <interface> promisc
```

## Snort configuration for specific IP & port

Snort isto only inspect packets destined for existing central servers:

* Use BPF (Berkeley Packet Filter) filters to restrict capture to specific IPs and ports.
* Define Snort rules to match traffic intended for the central servers.

### Option 1: BPF Filter (pre-capture filtering)

Capture only traffic going to the central server's IP (192.168.1.100) and port (80):

```
sudo snort -i eth0 -c /etc/snort/snort.conf -l /var/log/snort -A console -n 1000 -B "dst host 192.168.1.100 and dst port 80"
```

* dst host 192.168.1.100 → Filters for packets sent to the central server’s IP.
* dst port 80 → Only captures traffic for port 80 (HTTP).

### Option 2: Snort Rule (post-capture Filtering)

For more granular rule-based inspection, define a custom Snort rule in `/etc/snort/rules/local.rules`:

```
alert tcp any any -> 192.168.1.100 80 (msg:"HTTP Traffic to Central Server"; sid:1000001; rev:1;)
```

This rule triggers an alert for any TCP traffic going to 192.168.1.100:80.

## Verify & run Snort

Test Snort configuration:

```
sudo snort -T -c /etc/snort/snort.conf
```

Run Snort in IDS mode:

```
sudo snort -i eth0 -c /etc/snort/snort.conf -A fast -l /var/log/snort
```

## Port Mirroring/SPAN (If needed)

If the Snort box is on a separate switch port, ensure port mirroring (SPAN) is enabled to copy traffic from the central 
server's port to the Snort interface.

