## Setting up Windows 10/11 to remote VSCode to raspberry Pi 5 with an SSH key

Pages in this article:
- [Overview](./)
- [Connecting the Pi to the PC](rpi-connect-pc.md)
  - [Setup Direct Ethernet connection](rpi-vscode-ethernet)
  - [Setup USB Gadget connection](rpi-usb-gadget)
- [Setup SSH on the PC and Pi (this page)](rpi-ssh-vscode-setup)
- [Sample Python Project](python_sample_project)
- [EXTRAS- older Pis and Docker](rpi-vscode-extras.md)
<hr />
As noted in the overview, you will need to create an SSH key on your Windows PC that is shared to the Pi so that SSH authentication is password-free.  The essential steps are to generate the key on the PC, connect to the Pi with SSH (using your password) in your terminal, put the public key file on the Pi with appropriate permissions, then reconnect to the Pi without the password to verify.  Details are given below.  (Note, this has been tested on the Raspberry Pi 5 but may work on other models; "Pi" is used generically below).

### Assumptions:
* The Pi is setup on your local network with either a fixed IP address or is accessible with pi@hostname.local MDNS (note that while the standard Pi MDNS is raspberrypi.local if you change the machine name when the OS is configured it will be that name.local, like raspberrypi5.local).
* You are using the standard user "pi" on Pi, and a non-blank password.
* You are using the standard Windows 10/11 distribution that has the open SSH package installed (gives you SSH and ssh-keygen commands).  If you try to use SSH in the terminal and it fails, check that Open SSH is enabled in your PC settings; see the last URL in the reference list below.
* You have been able to SSH to the Pi from the win 10/11 powershell command line like `ssh pi@<pi-host-addr-or-name>` using the pi password.

### Directories used:
* Windows: `C:\Users\<user>\.ssh`  (this can be created with keygen)
* Pi: `~/.ssh` (create this manually using the install linux command per the instructions below)

### Prerequisites:
* Install latest VS Code on your windows PC.
* On the Pi, you can use either the full desktop or the lite version.  Further, you can either install full VS code on the Pi in full desktop, or let the VS code remote extension install services for you in either version (happens when you connect remote to the Pi). 
* If you don't install the desktop OS on the Pi, you need to install git support manually while connected with SSH to the Pi:
    1. First `sudo apt update`.
    2. Then `sudo apt install git`.
* You may also want to go ahead and set your git user and email; current versions of git won't let you commit without them:
    * `git config --global user.name "<your github username>"`
    * `git config --global user.email "<your github email>"`


### Steps
1. Per the VSCode article (2nd URL in the references below), add the Remote - SSH microsoft extension, configure the Pi as a host (like pi@raspberrypi.local or an IP) and remote into it with VSCode to make sure password auth is working to the VSCode server that will be installed on the Pi.
2. Per the same VSCode article, generate an SSH key on the windows machine.  In Powershell, type: 
`ssh-keygen -t ed25519`. DO NOT add a passphrase, just hit enter to leave it blank.  This will put files id_ed25519 and id_ed25519.pub in the `C:\Users\<user>\.ssh` directory.  The text contents of the id_ed25519.pub file is what will be copied to the Pi per the next step.
3. Per the "How to setup Raspberry Pi SSH..." article (1st URL in the references below), skip to the section **Copying the Public Keys Manually**.  Follow steps 1-5 to create the `~/.ssh` directory, put the public key from windows in the correct file, and set proper permissions. (* see below for a quick summary). *NOTE*, you can also use Powershell to push the SSH key from Windows to the Pi, see below.
4. Back on windows, you can first try to SSH from the terminal to the Pi; it should not prompt for any password since the key is being passed.
5. Finally in VSCode on windows do a remote-SSH connection to the Pi and verify you get in with no password (see the end of the [intro document](README.md) for details).

**NOTE** To revert to password login remove the file from `~/.ssh` on the Pi.  You can also remove the id_ed25519 files from the directory on the windows machine to keep it all clean.  Alternately, if you need to re-image your Pi but want to keep the SSH key you generated previously, just edit the "known_hosts" file in the `C:\Users\<user>\.ssh` directory to remove any line starting with "raspberrypi.local" or whatever name/IP you are using to SSH to the Pi.  Then you can SSH to the Pi using your username and password, then follow starting at step 3 above. 

**TLDR; Quick Steps From the SSH article for step 3 above**
1. install -d -m 700 ~/.ssh
2. nano ~/.ssh/authorized_keys
3. copy in the text from the .pub file of the SSH key, save and exit
4. sudo chmod 644 ~/.ssh/authorized_keys  
sudo chown pi:pi ~/.ssh/authorized_keys

**BONUS- Using Powershell to transfer the SSH key to the Pi**
You can use Powershell from the PC to push the SSH key to the Pi, including creating the ~/.ssh folder noted above.  The command below will also push a new key to an existing folder. This command assumes the ed25519 key is generated as noted in step 2 above.  The command is:

`type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh pi@raspberrypi5.local "umask 077; test -d .ssh || mkdir .ssh ; umask 022 ; cat >> .ssh/authorized_keys" || exit 1`


References:
* [https://pimylifeup.com/raspberry-pi-ssh-keys/](https://pimylifeup.com/raspberry-pi-ssh-keys/)
* [https://code.visualstudio.com/docs/remote/ssh-tutorial](https://code.visualstudio.com/docs/remote/ssh-tutorial)
* [https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh](https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh)
