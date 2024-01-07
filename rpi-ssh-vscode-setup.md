### Setting up Windows 10/11 to remote vscode to raspberry pi with an SSH key

Refs:
* https://pimylifeup.com/raspberry-pi-ssh-keys/
* https://code.visualstudio.com/docs/remote/ssh-tutorial

Assumes:
* Pi is setup on your local network with either a fixed IP address or is accessible with pi-hostname.local mdns (note that while the standard pi mdns is raspberrypi.local if you change the machine name when the OS is configured it will be that name.local, like raspberrypi5.local).
* using standard user "pi" on pi, and a non-blank password.
* you are using the standard windows 11 distribution that has the open ssh package installed (gives you ssh and ssh-keygen commands)
* you have been able to ssh to the pi from the win 11 powershell command line like `ssh pi@<pi-host-addr-or-name>` using the pi password.

Directories:
* Windows: `C:\Users\<user>\.ssh`  (this can be created with keygen)
* pi: `~/.ssh` (create this manually using the install linux command per the instructions below)

Prerequites:
* Install latest VS Code on your windows PC.
* On the Pi, you can use either the full desktop or the lite version.  Further, you can either install full VS code on the Pi in full desktop, or let the VS code remote extension install services for you in either version (happens when you connect remote to the pi). 
* If you don't install the desktop OS on the pi, you need to install git support manually:
    1. First `sudo apt update`.
    2. Then `sudo apt install git`.
* You may also want to go ahead and set your git user and email; current versions of git won't let you commit without them:
    * git  config --global user.name "<your github username>"
    * git config --global user.email "<your github email>"



### Steps
1. Per the vscode article, add the remote - ssh microsoft extension, configure the pi as a host (like pi@192.168.99.11) and remote into it with vscode to make sure password auth is working to the vscode server that will be installed on the pi.
2. Per the vscode article, generate an ssh key on the windows machine.  In Powershell, type: 
`ssh-keygen -t ed25519`. DO NOT add a passphrase, just hit enter to leave it blank.  This will put files id_ed25519 and id_ed25519.pub in the `C:\Users\<user>\.ssh` directory.  The text contents of the id_ed25519.pub file is what will be copied to the pi.
3. Per the "How to setup raspberry pi ssh..." article, skip to the section **Copying the Public Keys Manually**.  Follow steps 1-5 to create the `~/.ssh` directory, put the public key from windows in the correct file, and set proper permissions. (* see below)
4. Back on windows, you can first try to ssh from the command line to the pi; it should not prompt for any password since the key is being passed.
5. Finally in vscode on windows do a remote-ssh connection to the pi and verify you get in with no password.

**NOTE** to revert to password login remove the file from `~/.ssh` on the pi.  You can also remove the id_xxx files from the directory on the windows machine to keep it all clean.  Alternately, if you need to re-image your Pi but want to keep the ssh key you generated previously, just edit the "known_hosts" file in the `C:\Users\<user>\.ssh` directory to remove any line starting with "raspberrypi.local" or whatever name/IP you are using to SSH to the Pi.  Then you can SSH to the Pi using your username and password, then follow starting at step 3 above. 

**TLDR; From the ssh article**
1. install -d -m 700 ~/.ssh
2. nano ~/.ssh/authorized_keys
3. copy in the text from the .pub file of the ssh key, save and exit
4. sudo chmod 644 ~/.ssh/authorized_keys  
sudo chown pi:pi ~/.ssh/authorized_keys

