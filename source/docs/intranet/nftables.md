# NFTables

For NAT:

* The ARP address family only supports the input and output hook
* The bridge address family only supports the input, forward and output hook?
* Masquerading is a kind of source NAT.
* nftables masquerade will not work if iptables masquerade is in the kernel (unload or disable it).

```text
flush ruleset

table ip nat {
    # Destination NAT
    chain prerouting {
        type nat hook prerouting priority 100; policy accept;
        
    }
    # Source NAT. Masquerading is a kind of source NAT.
    # Can only be used in chains of type nat and only works in the output path.
    # Make sure masquerading is enabled in the kernel (true if using default kernel) 
    # nftables masquerade will not work if iptables masquerade is in the kernel (unload or disable it).
    chain postrouting {
        type nat hook postrouting priority 100; policy accept;        
        oifname {enp3s0, tun0} masquerade
    }
    
}
```

