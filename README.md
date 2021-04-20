# Assignment1-DiscoveringVMXFeatures
CMPE 283 Assignment 1
Eric Lin

1. I did this assignment by myself.

2. Steps taken to setup environment:

  Assignment 1:

  1. Download Ubuntu 20.04 LTS for Desktop

  2. Use Ubuntu .iso when creating a new VM on VMware

  4. When ask to update VM setting, leave all settings to default except for the hard disk space. Set disk space to 200GB. Not needed, but I also allocated 4 cores to the guest VM.

  5. Set the VMX feature on for the guest OS in order to make the MSR to work ###
    - Go to processor/memory in the Guest OS Settings
    - Enable hypervisor applications in this virtual machine
    - Enable IOMMU in this VM
    - If these settings aren't turned on, the code will return all 0x00 as the guest VM doesn't support VMX operation.

  5. Ran into a network issue regarding guest VM on VMware.  https://pupuweb.com/solved-fix-nat-not-working-intermittently-vmware-fusion-after-macos-update/
  
      When I ran sudo apt-get update, it was failing to download some of the updates which in turn makes "sudo apt install git fail".
    I would get strange errors when trying to install packages using "sudo apt install". For example when trying to download git I would get this error message: “The following packages have unmet dependencies: git : Depends: liberror-perl but it is not installable….E: Unable to correct problems, you have held broken packages.” Doing anything like "sudo apt update" or "apt-get dist-upgrade" followed by "apt-get -f install" doesn't work.
    I found out it was because of my network setting, I was using the bridge connection and it was recommended to use NAT. 

      However, when I try to use NAT, the ubuntu in the VM won’t connect.
    The solution was to restart the networking service on VMware using these commands:
    sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
    sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
    This ended up working and I was able to perform the apt update and git install successfully.

  6. Install git:
    sudo apt-get update
    sudo apt install git
    
  7. Build your environment referencing https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel. 
     This will install all tools such as "make" and gcc needed to build a kernel.
      - sudo apt-get build-dep linux linux-image-$(uname -r)
      - sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf

  8. Logged into GitHub in the guest VM. Create a new repository and git clone the repository into my VM.

  9. Forked and cloned the linux repository in the guest VM. This step is mainly for assignment 2.

  10. I then signed into canvas on the guest VM. I downloaded both the cmpe283-1.c file for intel and the Makefile from the CMPE 283 section in canvas. 
    I made my changes to the code to include all the MSRs for the VMX configuration. 
    To do that I referenced the Intel SDM for the processor, secondary process, exit and entry capabilities.
    I then edited the detect_vmx_features to report capabilities for the additional control types.
  
  11. cd into the assignment 1 repository. Run "make"

  12. To read the VMX configuration, run "sudo insmod cmpe283-1.ko"

  13. Then run "sudo dmesg" to view the output.

  14. To remove the kerbel module run "sudo rmmod cmpe283-1.ko".

  Assignment 2 Setup:

  1. cd to /linux on the VM
  2. run "uname -a" and note down the version. We will be using the same version config from the ubuntu boot file in the next step.
  3. run cp /boot/config-5.8.0-25-generic ./.config
  4. make oldconfig to update the config file. Hold enter to use all default values whenever the prompt ask for user inputs.
  5. make -j 4 modules && make && sudo make modules_install && sudo make install
      - this command took hours to complete so I left it running overnight
      - j 4 option is to specify that I want to use four cores to run the make modules
