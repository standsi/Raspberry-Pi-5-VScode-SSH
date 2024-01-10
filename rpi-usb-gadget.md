
### Connect Raspberry Pi 5 to PC with USB-C Gadget Mode

* setting up usb gadget *

- new image of os lite with no wifi
- connect pi with ethernet and turn on ics (assuming pi ethernet is 3):
disable-ics
set-ics "Wi-Fi" "Ethernet 3"

- setup pi for ssh per docs
*(clear raspberrypi5.local from known_hosts)
install -d -m 700 ~/.ssh
nano ~/.ssh/authorized_keys
* (copy in the text from the .pub file of the ssh key, save and exit)
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKrQrj/B4w2M/dcsz5xKXjj+eyMImPdIHZ+Jhur1TW/8 spadg@STAN-XPS13

sudo chmod 644 ~/.ssh/authorized_keys  
sudo chown pi:pi ~/.ssh/authorized_keys

- exit ssh and re-connect
- update pi
sudo apt update
sudo apt full-upgrade

- update firmware
sudo rpi-update
* (reboot)

- add kernal configs
sudo nano /boot/config.txt
dtoverlay=dwc2

sudo nano /boot/cmdline.txt
modules-load=dwc2

sudo nano /etc/modules
libcomposite

- contents of usb-gadget.sh to file
sudo nano /usr/local/sbin/usb-gadget.sh
* (put in contents)
sudo chmod +x /usr/local/sbin/usb-gadget.sh

- contents of usbgadget.service into file:
sudo nano /lib/systemd/system/usbgadget.service
* (put in contents)
sudo systemctl enable usbgadget.service

- setup network bridge - **** note leaving off last line from article, not setting static ip
sudo nmcli con add type bridge ifname br0
sudo nmcli con add type bridge-slave ifname usb0 master br0
sudo nmcli con add type bridge-slave ifname usb1 master br0

- install dnsmasq, config
sudo apt-get install dnsmasq
sudo nano /etc/dnsmasq.d/br0

dhcp-authoritative
dhcp-rapid-commit
no-ping
interface=br0
dhcp-range=10.55.0.2,10.55.0.6,255.255.255.248,1h
dhcp-option=3
leasefile-ro

- shut down the pi and remove ics
sudo shutdown now
disable-ics

- connect laptop to pi with usb c, wait until usb errors and then comes back up as rndis, then connect ics
get-netadapter
set-ics "Wi-Fi" "Ethernet 6"

- connect to pi and verify
ssh pi@raspberrypi5.local
ping www.google.com

!!!!success !!!







