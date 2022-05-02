# Setup Ubuntu 22.04 as bridged wireless access point and get the IP's from the main DHCP router / device
Ubuntu create a bridged access point with hostapn (01.05.2022)

**1. Install Ubuntu 22**

**2. Update Ubuntu and install all software**
```
sudo apt-get update -y && sudo apt-get upgrade -y 
sudo apt-get install bridge-utils
sudo apt-get install hostapd
sudo apt-get install ifupdown
```

**3. Find out your WLAN interface**
```
ip add
```

**4. Edit the hostapd.conf**
Fill out you **Your-WLAN-Interface** and **PASSWORD** in hostapd.conf. 
No spaces in the .conf file.
```
sudo nano /etc/hostapd/hostapd.conf
```

These are my settings, insert this text
```
interface=Your-WLAN-Interface
driver=nl80211
bridge=br0
ssid=WLAN
hw_mode=g
ieee80211n=1
channel=1
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=Your-WLAN-PASSWORD
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

**5. Edit the start up of hostapd**
```
sudo nano /etc/default/hostapd
```
Add this line or edit it
```
DAEMON_CONF=/etc/hostapd/hostapd.conf
```

**5. Edit interfaces**
```
sudo nano /etc/network/interfaces
```
Insert this text
```
auto br0
iface br0 inet dhcp
bridge-ports Your-ETHERNET-Interface Your-WLAN-Interface
```

**6. start hostapd**
```
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
sudo systemctl start hostapd
service hostapd start
```
**7. reboot**

**optional**
```
sudo apt-get install gnome-tweaks 
sudo apt-get install gnome-shell-extensions gnome-shell-extension-desktop-icons-ng
```

**Helpful**
https://wiki.gentoo.org/wiki/Hostapd
