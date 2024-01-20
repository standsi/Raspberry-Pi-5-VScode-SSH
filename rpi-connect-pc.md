## Methods for Connecting the Pi to the PC

Pages in this article:
- [Overview](./)
- [Connecting the Pi to the PC (this page)](rpi-connect-pc.md)
  - [Setup Direct Ethernet connection](rpi-vscode-ethernet)
  - [Setup USB Gadget connection](rpi-usb-gadget)
- [Setup SSH on the PC and Pi](rpi-ssh-vscode-setup)
- [Sample Python Project](python_sample_project)
<hr />
The Pi can be connected to your PC a number of different ways depending on your network, hardware, etc.  Please note, power to your Pi will generally be best configured as a separate USB-C connection except for the "all in one" method #4.  While you could supply power from your laptop the USB-C port on the Pi requires a USB Power Delivery supply with sufficient current (such as Thunderbolt).

1. ***Connect Pi to the same wireless or LAN to which your PC is connected***; the network usually contains a router giving internet access along with DHCP address management.  This is the easiest to setup if available: when you image the OS for the Pi configure a wireless SSID and password (or just plan to use Ethernet if available, which is on and setup for DHCP by default).  
This is an out-of-the-box setup on both the Pi and the PC, requiring no special networking changes.  (Note that in the Pi5 OS SSH is enabled by default.) But as noted above you may not want to connect the Pi directly to your network, or you may not have a network if you simply have your PC connected to a wifi hotspot but can't have the Pi also connected.  (When you first go through the [step 2 document](rpi-ssh-vscode-setup.md) it will be easiest to use this connection type even if you change it later for daily use)
2. Another approach which alleviates the potential public network exposure of your Pi is to ***use a small router often called a travel router***.  This forms a small private network that shares an internet connection, most offering either wireless or Ethernet connections to both the internet and device sides.  This is another out-of-the-box solution for both the Pi and PC, but does require a hardware purchase and configuration.  
For the modest bandwidth needed for remoting to your Pi, well-rated devices typically sell for under $50.  This [device](https://a.co/d/faXdwOX) was tested; it was easy to setup and performed well.  Once the router is connected to the internet the setup is the same as #1 above: connect the PC and Pi either through wireless or Ethernet and both should have internet connectivity.
3. Another approach which offers a private network for your Pi but doesn't require expensive hardware is ***using a direct ethernet connection***.  This method is out-of-the-box for the Pi but does require some minor networking configuration on the PC.  The PC can share its internet connection whether connected wired or wireless (if wired, two ethernet connections are required on the PC).  
**For a full guide on implementing this technique see [this document](rpi-vscode-ethernet.md).**
4. Finally, there is an all in one solution that ***powers the Pi and sets up all the communication needed***.  This uses a feature on the Pi called USB gadget mode.  As noted above, this requires a PC (typically a laptop) with a USB-C Power Delivery port capable of at least 3A of current (such as offered typically by Thunderbolt).  The only hardware needed is a USB-C to USB-C cable to connect the PC to the power port on the Pi.  This is not an out-of-the box solution on either side; both the Pi and the PC will require networking configuration changes.  But the advantage is a very compact setup, such as for traveling.  
**For full details see [this document](rpi-usb-gadget.md).**  
**NOTE: BUG BEFORE LATE DECEMBER 2023**  
There WAS a [reported issue](https://github.com/raspberrypi/bookworm-feedback/issues/77) preventing the Pi 5 from properly loading the driver required to emulate a network connection on the USB-C power port.  This existed on the Pi 5 delivered in 2023, but it is not clear when/if it was fixed in production.  There was a patch put out to the firmware in late 2023 that seems to work.  The document includes instructions for manually updating the firmware on your Pi. If you have a more recently produced Pi (look on the back for a code like YYWW where WW is the week of the year), try the setup without the firmware update to see if the fix is in production.
