# Flash router with other firmware

Routers supplied by ISPs are typically less secure than those sold by manufacturers to consumers. They often have hard-coded remote support credentials that users can't change and patches for their customized firmware versions lag behind patches for the same flaws released by router manufacturers. There are several Linux-based, community-maintained firmware projects for a wide range of home routers. OpenWRT, DD-WRT and Asuswrt-Merlin (for Asus routers only). But, loading custom firmware on a router requires some technical knowledge, will likely void its warranty and, if done incorrectly, can brick the device.

## DD-WRT

DD-WRT has become a common out-of-the-box option for many routers, but also exists in stand-alone implementations that can be used to flash routers that support it. It has a slightly convoluted history. From 2002, Linksys released a line of routers (WRT54G) with Linux. The company was eventually obliged to release the source code for those routers under the terms of the GPL. DD-WRT has become the basis for other firmware created by router manufacturers themselves and while DD-WRT is released under the terms of the GPL, commercial builds of such firmware may incorporate much non-GPL code. 

## OpenWrt

OpenWrt is more like a real Linux distribution than DD-WRT. It comes with its own package manager. Setting up and running OpenWrt can be an involved process, because users can make any changes they want from a broad range of components directly inside OpenWrt. Updates come frequently and its package manager makes it easy for users to take advantage of those updates. 

### Example: Flashing a TL-WR841N(D)

Read [Factory install: First-time installation on a device](https://openwrt.org/docs/guide-quick-start/factory_installation). 

For showing how easy it is:  

* Look on the bottom of your router. There is a version number on the label. In my case at the time of writing that was 9.2.
* Go to OpenWrt and find the [TL-WR841ND software](https://openwrt.org/toh/tp-link/tl-wr841nd) (note that it is now no longer recommended to flash this type of router, but for 9.x a version is available that still works). On that page you can also read the latest notes on the versions, with links to bugs and discussions. Download the image for your version. 
* Open the router web interface page on `tplinklogin.net` or `192.168.1.1`. If you never changed your password, tsk, tsk, it is [default user : admin, password: admin]. If you forgot, [reset the router](https://www.tp-link.com/en/support/faq/497/) and then login with the defaults (in which case you can skip the next step). If you did have a password set, log in.

* Do a factory reset after taking a look at your current settings.
* Go to the firmware upgrade page and select the firmware image you just downloaded as an update. 
* Upgrade and set a password

Done.


