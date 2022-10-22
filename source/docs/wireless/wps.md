# Notes on WPS

The Wi-Fi Protected Setup (WPS) protocol in home routers is like labelling it "HACK ME!". WPS was a bad idea to begin 
with, has a big design flaw in PIN authentication, comes from a source that is not trusted, is very complicated and 
as a result there have been multiple instances of poorly written, buggy implementations. The design flaw in the WPS 
specification for PIN authentication significantly reduces the time required for an adversary to [brute force an 
entire PIN](red-network:docs/wireless/wps-pin), because it informs them when the first half of the eight-digit PIN is 
correct. Many routers lack a lockout policy after a certain number of failed attempts to guess the PIN, making a brute-force attack much 
more likely to occur. 

## Check for WPS 

The feature that scans for nearby networks on operating systems like OS X, iOS, Windows, Android and Chrome OS does 
not report on WPS. 

* On Windows, the free and portable [WifiInfoView](https://www.nirsoft.net/utils/wifi_information_view.html) includes reporting on WPS. When WPS is enabled, the status is either Configured, Not Configured, or Locked. A value of "No" means WPS is not enabled. 
* On Android, the [Wi-Fi Analyzer app](https://play.google.com/store/apps/details?id=com.farproc.wifi.analyzer&hl=en) shows if WPS is supported for each SSID detected by displaying an On/Off status. If WPS is enabled, it displays "WPS", otherwise it displays nothing.

## Warnings

* Wi-Fi Direct uses WPS to make the connection using either Near Field Communication, a PIN, Bluetooth or a button press.
* [How seven mesh routers deal with Wi-Fi Protected Setup (WPS)](https://www.computerworld.com/article/3193163/networking/how-seven-mesh-routers-deal-with-wps.html)

