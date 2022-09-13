# Services audit

## Questions

  * Is this service really necessary?
  * Is the service running on interfaces that it doesn't need to? Is it better to bound it to a single IP?
  * Do the firewall rules allow legitimate traffic to pass through to this service?
  * Do the firewall rules block traffic that is not legitimate?
  * Is there some way security alerts about vulnerabilities for each of these services can be received?

Use `netstat` (if not available install net-tools):

    $ sudo netstat -plunt

Pay attention to `Proto`, `Local Address`, and `PID/Program` name. If the address is `0.0.0.0`, then the service is accepting connections on all interfaces. 

## Disable services when not needed

Make services like Telnet, Universal Plug and Play (UPnP), Secure Shell (SSH), and Home Network Administration Protocol (HNAP) unreachable from the internet. Turn them off on the local network if they're not needed. 

Universal Plug and Play (UPnP) is a handy feature that allows networked devices to seamlessly discover and establish communication with each other on the network. The UPnP feature eases initial network configuration and is also a security risk. Malware within a network can use UPnP to bypass a router’s firewall and allow attackers to take control of devices remotely to spread malware to other devices. 

Get your router's IP address

    $ host myip.opendns.com resolver1.opendns.com

[Scan your router's public IP address](cheatsheets:docs/scanning/Nmap-cheatsheet) for open ports.
