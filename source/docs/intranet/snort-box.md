# Snort box

The idea is to add another server, a snort box (IDS server) that is actually another central server (yet has no IP address) and copy each packet intended for already existing central servers (if any). 
* Promiscuous mode
* Rule: Only listen for this IP address on this port (of existing central server)
