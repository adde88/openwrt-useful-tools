# OpenWRT 19.07 - Useful Tools 

## (For WiFi Pineapples *v2.6.0 or above*)
---
Here you will find ALOT of different tools (both binaries and installation-packages) that i consider useful!  
There are seperate branches for different versions of OpenWRT (Hak5 firmwares)  
Packages for the older firmwares can be found at the **[packages-15.05](https://github.com/adde88/openwrt-useful-tools/tree/packages-15.05)** branch of this repo, which will also continue on getting updates!  

---
### Add my repo to OPKG:
Do you want to make your Pineapple automatically get my signed packages through **OPKG** from my private repo without downloading them here?  
Then SSH to your Pineapple and add the lines below to: **```/etc/opkg/customfeeds.conf```**
```
# Zylla Custom Packages (Bleeding Edge)
src/gz 1907_custom http://adde88.asuscomm.com:1080/packages/mips_24kc/custom
```

**Andreas Nilsen** [@adde88](https://twitter.com/adde88)

---
#### Donations:
Are very much appreciated!  
Alot of my spare-time goes to keeping software up-to-date with the Pineapples, and helping out the community in general!  
It really helps me so i can continue on with this work. :)


[![paypal](https://www.paypalobjects.com/en_US/NO/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=4HJM939H9PHWW)
