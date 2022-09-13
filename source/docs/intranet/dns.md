# DNS cache poisoning

* Do domain name server audit trails to investigate unauthorised use and DNS cache poisoning.
* Monitor traffic to create whitelists of valid DNS servers (for example by using DNS metadata (server source, destination IP address, domain names that were queried by the server, and DNS server registration data)) and be alerted if servers not on the list are used. 
* Use whitelist in a firewall or network access control system (NAC)

Monitoring:
* Most firewalls can not function as general anti-DNS-spoofing tool, but can inspect all DNS traffic for unauthorised byte patterns blocking particular name server software exploit attacks (such as DNSChanger, OSX/MaMi) and then shut down as specified. 
* Traffic analyser scripts can be made to search for suspicious file activity in DNS traffic between clients and resolvers.
* Resolver log analysis can be monitored to find unauthorised or malicious domains. 
* Intrusion Detection Systems can not mitigate the effects of the attack, but they can identify suspicious traffic patterns.
* Passive DNS replication data can be used for identifying unauthorised DNS services. Passive DNS data is rich, and can be used to help administrators block the resolution of suspicious new domain names, identify potential infringements and as source for threat intelligence.

