# ARP spoofing

* Use static, read-only entries for critical services in the ARP cache of a host (prevents only simple attacks and does not scale).
* Switches do cross-checking of ARP responses (combined with certification).
* OS of devices ignore unsolicited replies. The different OS's respond differently:
    * Linux ignores, but accepts responses to requests from other machines to update its cache.
    * Windows ARP cache can be configured through registry entries.
* Packet filtering: a spoofed packet can contain packets from outside the network that shows source addresses from inside the network and vice-versa.
* Authentication and encryption
* The above mitigations can only prevent simple ARP attacks. Use anti-ARP tools to identify and stop the more sophisticated adversary.
