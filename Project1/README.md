# Project 1
## Part 1
This guide will go over how to setup a Linux Mint Cinnamon virtual machine using VirtualBox.

Guest VM used: Linux Mint Cinnamon
VMM (virtual machine manager / software): VirtualBox

### Setting up Linux Mint on a Windows 10 Host. 
1.	Download the Linux Mint Cinnamon Edition ISO from https://linuxmint.com/download.php
  a.	It is strongly recommended to move this ISO out of the downloads folder and into a more permanent folder. VirtualBox will point to it later, so it is best to avoid moving it around. 
2.	In VirtualBox, click "new" to begin creating a new machine.
  a.	The "machine type" should be set to Linux, and the "version" should be set to Ubuntu (64-bit) as this is the closest option to Mint. 
  b.	Allocate at least 8192MB of memory.
  c.	Create a “virtual hard disk” for the VM to store stuff on (click next).
  d.	Use a VDI for the virtual hard disk (click next).
  e.	Select “Dynamically allocated” for storage and allocate 350GB of space.
  i.	I did it this way so I could use the VM for multiple different things without having to worry about running out of space. I have a 2TB SSD where the VM is located. 
3.	Now that the machine is created, click on settings and tweak a few things. 
  a.	Go into “Display” settings and make sure 3D acceleration is turned OFF. There is currently a bug within the newest version of Mint that makes the screen white if 3D acceleration is enabled. More information can be found at https://forums.linuxmint.com/viewtopic.php?t=354405.
  b.	Under "system settings", click on "processor" and slide it to 4 CPUs. This is in terms of threads, so 2 cores will be allocated to the VM. 
4.	Navigate to "storage" under settings. Select the empty disk icon under the IDE controller. On the right-hand side under attributes, click the disk icon and then click “choose a disk file”. Navigate to where the ISO is located (I recommend moving it out of the downloads folder) and select it. 
5.	Click start to start the Mint VM.
  a.	Wait for the timer displayed by the OS to reach 0 (or hit enter over “Start Linux Mint”).
  b.	Double click the CD on the desktop of the guest that says “Install Linux Mint”. 
  c.	Choose your language and keyboard layout, clicking continue where appropriate. 
  d.	Check the option to “install third-party software for graphics and…”. NOTE: The wording and structure of this step changes from time to time. Make sure you’re selecting the option(s) installing software for graphics, wi-fi, flash, mp3, other media, etc.  
  e.	Select “Erase disk and install Linux mint”. This will not erase your host OS/files, only the guest’s (which is “empty” at this stage anyway). 
  f.	Click “Install Now” and “Continue”. 
  g.	Select your region, click continue.
  h.	Name the computer and yourself, then create a password.
  i.	Once Mint tells you the installation has finished, click restart now. 
    i.	Press “Enter” when it says to remove the installation media, WITHOUT removing it. Mint operates off of one ISO. 

You cannot resize the window of the VM initially. To enable this, you must install VirtualBox Guest Additions. 
1.	In the VirtualBox window, click on “Devices” and select “Insert Guest Additions CD Image”. 
2.	Click Run. 
3.	Enter your password and click on “Authenticate”. 
4.	Once the install has finished, press enter and restart the VM. The window should now resize to fit your host’s monitor. 

## Part 2 – Exploring Virtualization
### Exploring Host Disk Usage
1.	The Mint VM is saved to my D drive and takes up 44.5GB of space currently, and it can reach a maximum of 350GB. 
2.	You cannot access files and folders created on this VM by navigating through the host’s filesystem to where the VM is stored. The VM is stored as one big VDI file. If you go to settings, general, and then advanced, you can choose to configure how moving files works between the host and guest. 
3.	When taking a snapshot, a .VDI file and a .sav file are created. The sav file is 672MB and the VDI file is 66MB. To take a snapshot you go to the “Machine” drop down at the top of where your guest OS is running. Then click “take snapshot”. Enter a name and description for the snapshot if you wish. 
4.	VirtualBox’s version of templates is to create a “clone”. Right click the machine you want to create the template from, and click on clone.
a.	Change the path and name values if necessary. 
b.	Generate new MAC addresses for all network adapters. 
c.	Click Next. This is the simplest way (in my opinion) to create a clone that will be separate from the original but with the same setup. 
d.	I chose to do a “full clone” including “current machine state”. I have a lot of timeshift backups in the guest-to-be-cloned, but only one recent snapshot. 
e.	The clone is the exact same size as the original, and has all of the same files on the host except the “Snapshots” folder.   
### Exploring Guest Networking
My host is connected to a router via ethernet. The router has a public IP address assigned to it. Each device connected to the router gets a private IP address (including my host). The router is able to tell which device is which thanks to Network Address Translation. It translates the private IP into a public one in order to pass the traffic from the device to the internet using the router’s public IP and an ethernet connection with the modem. 

The guest connection is very similar, but with the addition of a hypervisor. The guest has a private IP address that is structured differently from the private IP address of the host’s router. The host acts as the router for the guest (thanks to the hypervisor). Therefore, the guest uses the same public IP as the host in the end. Traffic travels from the guest to the host (if it meets the firewall/security rules) and then to the router. 

## Part 3 - Networking with Style
I chose to experiment with host only networking. 
In VirtualBox, select "settings", "network", "attached to", and the pick "Host-only Adapter" from the dropdown. There was only one "Name" available, so select VirtualBox Host-Only Ethernet Adapter.
The private IP address for my guest was in the 10.0.0.0/24 format while using NAT networking, and switched to 192.168.56.0/24 format when in host-only mode. You can ping the private IP of your guest from your host to test if it is in NAT mode or another kind of mode. Pings won't work in NAT mode, but they will in host only (and bridged) mode. 
## Useful commands:
### Powershell:
- `ipconfig` - Displays settings for all available networking adapters
### Linux / bash:
- `ip a` - Displays settings for all available networking adapters
