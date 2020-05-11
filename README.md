## FastgateRoot
How to get full access to your Fastgate, a widely used Router distributed by the ISP Fastweb in Italy.

## How does it work?
This trick uses a very old flaw available in basically every Technicolor router, you can have a USB return the root folder of the router by crafting a special symbolic link. 

## Getting Started

### Requirements

* A FASTGate DGA4131 (VBNT-O).
* An USB Drive.
* A Linux machine (a live version of Ubuntu should be ok, probably WSL too).

### Instructions

* Open up a terminal, navigate to the USB Drive path and type the following: `ln -s / rootlink`.
* Open up a browser, login to your fastgate admin page (it's usually available [here](http://192.168.1.254)).
* Once you logged on, click on this [link](http://192.168.1.254/status.cgi?3g_pin=********&act=nvset&samba_enabled=1&samba_workgroup=WORKGROUP%5c%0a%09security%20%3d%20share%5c%0a%09guest%20account%20%3d%20root%5c%0a%09interfaces%20%3d%20lo%20br-lan%5c%0a%5c%0a%5bohnonotagain%5d&service=usb_status).
* Plug the USB Drive in your router.
* Navigate to `\\192.168.1.254\usbdisk\rootlink` by using a SMB Client (like Windows Explorer).
* Now you should be able to access the FS of your router, there are still a few things to do though.
* Edit `/etc/inittab` by removing '#' from the last line `::askconsole:/bin/login`, if there's none just leave it as it is. This should turn the console login on, in case you screw your router heavily you should be backed up now.
* Edit `/etc/passwd` by removing `root:/bin/false` and adding `root:/bin/ash`. This allows using a shell for an eventual login.
* Edit `/etc/config/dropbear` as following (delete every other config involving SSH in this file, if there's any):
`config dropbear 'Example'
 option Interface 'lan'
 option Port '22'
 option IdleTimeout '600'
 option PasswordAuth 'on'
 option RootPasswordAuth 'on'
 option RootLogin '1'
 option enable '1'`
*


