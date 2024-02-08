## EXTRAS - Using Older Pi's and Docker on Pi

Pages in this article:
- [Overview](./)
- [Connecting the Pi to the PC](rpi-connect-pc.md)
  - [Setup Direct Ethernet connection](rpi-vscode-ethernet)
  - [Setup USB Gadget connection](rpi-usb-gadget)
- [Setup SSH on the PC and Pi](rpi-ssh-vscode-setup)
- [Sample Python Project](python_sample_project)
- [EXTRAS- older Pis and Docker (this page)](rpi-vscode-extras.md)
<hr />
Several additional topics came up during the review of this article that might be of interest.

1. How well does VSCode remote work with older Pi's like the 4, 3 and even Zero?
2. Does Docker run on the Pi 5 and can it be used with VSCode Dev Containers?  If so, is that even useful?

### VSCode Remote Dev with older Raspberry Pi's




----------

tried connection methods (link) 1,2,3 on pi 4 and 3, 1 only on pi 2w.

all images were lite, pi 4 and 3 were 64 bit of the latest OS available for that device, zero 2w was tried at 64 bit but too big, worked at 32 bit

pi 4,4GB, ethernet direct, see htop image, ethernet name resolution was slow (5 secs to start pinging google) *** internet issue??

pi 3, 1GB, ethernet direct, also slow name resolution *** internet issue???, see htop image

pi2w, 512MB.  Wifi (no ethernet adapter), able to connect with vscode, load extensions (python), run a small program.  memory usage kept climbing as extensions came into play, response was laggy, but did not crash.  see htop images.



