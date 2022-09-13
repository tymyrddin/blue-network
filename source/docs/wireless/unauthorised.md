# Unauthorised devices

Many routers allow for restricting which devices are allowed on the Wi-Fi network based on their MAC address. Enabling this feature can prevent attackers from connecting to a Wi-Fi network even if they stole its password. On larger networks this must be scripted for it becomes quickly undoable. 

If a router does not have this feature, you can maybe use the router manufacturer’s website to monitor for unauthorised devices joining or attempting to join your network, or install a tool like [Kismet Wireless](https://www.kismetwireless.net/) on a for the purpose dedicated device.

* Rogue WAPs: Immediately block the IP address of the WAP device at the switch it is connected to. This will probably give enough time to find the physical device while the rogue is trying to discover what happened to his or her wireless network connection.
* Non-wireless devices: If you find unknown non-wireless devices such as printers, servers, etc. immediately do an in-depth scan (for example by using [Ettercap](http://www.ettercap-project.org/)) and determine exactly what the device's function is. Block the device from the network until you can physically locate it and disconnect it.

