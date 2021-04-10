# Assignment1-DiscoveringVMXFeatures
CMPE 283 Assignment 1

1. I did this assignment by myself.

2. Steps to setup environment.

Step 1: VMware Fusion on Mac

1. Download Ubuntu 20.04 LTS for Desktop
2. Use Ubuntu .iso when creating a new VM on VMware 
3. When ask to update VM setting, leave all settings to default except for the hard disk space. Set disk space to 200GB.

4. Ran into a network issue regarding guest VM on VMware.  https://pupuweb.com/solved-fix-nat-not-working-intermittently-vmware-fusion-after-macos-update/

At first I try to run sudo apt-get update but this was failing to download some of the updates which in turn makes sudo apt install git fail.
I would get strange errors from the git download such as: “The following packages have unmet dependencies: git : Depends: liberror-perl but it is not installable….E: Unable to correct problems, you have held broken packages.”
I found out it was because for my network setting, I was using the bridge connection and it was recommended to use NAT.

However, I was trying to use NAT but ubuntu in the VM won’t connect.
On my MacBook I opened the terminal and ran:
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
in order to restart the networking service for VMware
This ended up working and I was able to perform the apt update and git install successfully.

4. Install git
sudo apt-get update
sudo apt install git



sudo add-apt-repository



- run “sudo apt-get update && sudo apt-get dist-upgrade && apt-get -f install”
# git needs liberror-perl as a dependency
- run sudo apt install perl
- run sudo apt install perl-base

# install liberror-perl 

5. Logged into GitHub in the guest VM and git cloned the forked linux repository into my VM.

6. I then signed into canvas on the guest VM. I downloaded both the cmpe283-1.c file for intel and the Makefile from the CMPE 283 section in canvas.
