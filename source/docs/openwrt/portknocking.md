# Portknocking

Portknocking is establishing a host-to-host communication across closed ports by means of "secret knocks". Data is transmitted to closed ports and once a monitoring daemon detects an agreed upon secret knock, the device unlocks the door and intercepts the information without sending a receipt to the sender. The information can be encoded and encrypted, into a sequence of port numbers.

For example, after receiving pre-specified sequenced connection attempts on a set of closed ports, a daemon of the OS on the router adds a host source IP to the allowed address list and the sender (client) can connect to the router. 

On OpenWrt, the Linux package ''knockd'' can be used. Knockd is a port knocking daemon, that listens for specific packets on specific ports, and will run a command when it hears the correct sequence. It is used to hide ports from public view for better privacy/security. And it isn't. Its a security hole used by malware.

## Resources

* [Portknock server](https://openwrt.org/docs/guide-user/services/remote_control/portknock.server), OpenWrt
* [How to Use Port Knocking on Linux (and Why You Shouldn’t)](https://www.howtogeek.com/442733/how-to-use-port-knocking-on-linux-and-why-you-shouldnt/)




.
