# Assignment1-DiscoveringVMXFeatures
CMPE 283 Assignment 1

1. I did this assignment by myself.

2. Steps taken to setup environment.

  1. Download Ubuntu 20.04 LTS for Desktop
  2. Use Ubuntu .iso when creating a new VM on VMware 
  3. When ask to update VM setting, leave all settings to default except for the hard disk space. Set disk space to 200GB. Not needed, but I also allocated 4 cores   to the guest VM. 
  4. Set the VMX feature on for the guest OS in order to make the MSR to work ###
    - Go to processor/memory in the Guest OS Settings
    - Enable hypervisor applications in this virtual machine
    - Enable IOMMU in this VM
    - If these settings aren't turned on, the code will return all 0x00 as the guest VM doesn't support VMX operation.

  5. Ran into a network issue regarding guest VM on VMware.  https://pupuweb.com/solved-fix-nat-not-working-intermittently-vmware-fusion-after-macos-update/

  At first I try to run sudo apt-get update but this was failing to download some of the updates which in turn makes sudo apt install git fail.
I would get strange errors from the git download such as: “The following packages have unmet dependencies: git : Depends: liberror-perl but it is not installable….E: Unable to correct problems, you have held broken packages.” Doing anything like apt update or apt-get dist-upgrade followed by apt-get -f install doesn't work.
I found out it was because of my network setting, I was using the bridge connection and it was recommended to use NAT. 

However, when I try to use NAT but ubuntu in the VM won’t connect.
On my MacBook I opened the terminal and ran:
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
in order to restart the networking service for VMware
This ended up working and I was able to perform the apt update and git install successfully.

4. Install git:
  sudo apt-get update
  sudo apt install git

sudo add-apt-repository

- run “sudo apt-get update && sudo apt-get dist-upgrade && apt-get -f install”

5. Logged into GitHub in the guest VM. Create a new repository and git clone the repository into my VM.

6. Forked and cloned the linux repository in the guest VM. This step is mainly for assignment 2.

7. I then signed into canvas on the guest VM. I downloaded both the cmpe283-1.c file for intel and the Makefile from the CMPE 283 section in canvas. I made my changes to the code to include all the MSRs for the VMX configuration.
8. cd into the assignment 1 repository. Run "make"
9. To read the VMX configuration, run "sudo insmod cmpe283-1.ko"
10. Then run "sudo demsg" to view the output.
11. To remove the kerbel module run "sudo rmmod cmpe283-1.ko".

Assignment 2 Setup

1. cd to /linux on the VM
2. run cp /boot/config-5.8.0-25-generic ./.config
3. make oldconfig. Hold enter to use all default values
4. make -j 4 modules && make && sudo make modules_install && sudo make install
    - this command took hours to complete so I left it running overnight
    - j 4 option is to specify that I want to use four cores to run the make modules
