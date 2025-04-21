# Enabling DoH/DoT in browsers

```{contents} Table of Contents
:depth: 3
```

## Firefox

Enable DoH:

* Go to Settings → Network Settings → Enable DNS-over-HTTPS.
* Choose a provider (Cloudflare, NextDNS).

Force DoH (about:config):

```
network.trr.mode = 3  # Force DoH
network.trr.uri = "https://cloudflare-dns.com/dns-query"
```

## Chrome/Edge

Enable DoH:

* Navigate to chrome://flags/#dns-over-https → Enable.
* Select a provider (Google, Cloudflare).

## System-Wide DoT (Linux/macOS)

Configure systemd-resolved:

```
sudo nano /etc/systemd/resolved.conf
```

Add (ini):

```
[Resolve]
DNS=1.1.1.1#cloudflare-dns.com
DNSOverTLS=yes
```

Restart:

```
sudo systemctl restart systemd-resolved
```

## Verify DoH/DoT

Check DNS encryption:

```
 # Linux
 sudo tcpdump -i eth0 -n port 53  # Should show NO plaintext DNS
 # Windows
 Wireshark filter: `dns && !(tls || http)`
```

Test with Cloudflare’s checker:

```
curl -s https://1.1.1.1/help | grep "DNS over HTTPS"
```

