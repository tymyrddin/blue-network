# Protect router management

Most routers offer the option to view and modify their settings over the internet. For most users, managing the router from outside the home network is not necessary. Turn this feature off if not needed. If remote management is needed, consider using a Virtual Private Network (VPN) solution to establish a secure channel to the local network first and then access the router's interface.

Even inside the home network, if possible, restrict which IP (Internet Protocol) addresses can manage the router. If this option is available, it's best to allow access from a single IP address that is not part of the pool of IP addresses assigned to computers via DHCP (Dynamic Host Configuration Protocol). For example, configure the router's DHCP server to assign IP addresses from `192.168.0.1` to `192.168.0.50` and then configure the web interface to only allow access from `192.168.0.53`. That computer can then be manually configured to use that address only when it is to connect to the router.

If not using command-line, turn on HTTPS access to the router interface, if available, and always log out when done. Use the browser in `incognito` or `private` mode when working with the router so that no session cookies are left behind and never allow the browser to save the router's username and password.
