# Constructing-a-Home-based-Cybersecurity-Lab-for-Detection-and-Surveillance

# Abstract
The project outlines the creation of a cybersecurity homelab focused on detection and monitoring. It provides a practical environment for configuring, securing, and monitoring an IT infrastructure, enabling users to apply their skills in real-world scenarios. Components include pfSense firewall, Security Onion IDS, Kali Linux for attacks, Windows Server as a domain controller, Splunk for log management, and various Linux machines. The project emphasizes hands-on learning and safe experimentation in cybersecurity practices.

# What is a Homelab?
A Homelab is an environment meant to simulate enterprise components and configuration. The goal here is to understand the process of installing, configuring, maintaining and updating this entire infrastructure. We are building a virtualized Homelab as it is beginner friendly and easier to configure and spin up.

# Tools
+ Attacker - Kali Linux as an attack machine
+ Firewall - pfSense
+ IDS - SecurityOnion
+ SIEM - Splunk
+ Hypervisor - VMWare/Virtualbox
+ Domain Controller - Windows Active Directory
+ Vulnerable Machines - Ubuntu, Windows, DVWA, VulnHub machines

# Network Design
![](https://github.com/kaddiri/Constructing-a-Home-based-Cybersecurity-Lab-for-Detection-and-Surveillance/blob/main/GIF.gif)

# Guide
# Selecting and Downloading a Hypervisor
We will need a Hypervisor to install all of our tools and services.

I'll be using VMWare Workstation, being a student I get a license for it for free. Although VirtualBox is also a great free alternative as a Hypervisor.

You can download VMWare Workstation at -

[VMWare Workstation Download] (https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)

You can download VirtualBox at -

[Virtualbox Download] (https://www.virtualbox.org/wiki/Downloads)

# Installing and Configuring the Firewall - pfsense
pfsense will act as the edge of our Homelab virtual network and will be only accessible from the Parrot Machine.

pfsense community edition can be downloaded from - [pfsense community edition ISO] (https://www.pfsense.org/download/)

You can follow these steps to install & configure pfsense -

1. Open VMWare Workstation & Create a new Virtual Machine with the "Typical (recommended)" setting.

2. Browse the pfsense CE ISO file and select Next.
(You might need to extract the ISO file from your pfsense download as it is usually zipped.)

pfsense New VM

3. Change your VM name to "pfsense" & click Next.

4. Leave the disk size to 20GB and ensure split virtual disk into multiple files option is selected.

pfsense Disk Size

5. Click on customize hardware and increase the memory limit to 1GB.

6. Add 5 Network Adapters and correspond them with a VMnet interface as per the image by selecting Custom specific Virtual Network.

pfsense Network Adapters

7. Select Finish. The pfsense machine will power on and you can accept all the default values, after that pfsense will boot.

8. Press Install, and select all the default configurations.

pfsense Installer

9. After the pfsense is done rebooting you will reach this screen.
pfsense Configuration

10. Select option 1 to set up the VLAN. Follow by -
'Should VLAN's be set up now [y|n]?' - n

Enter the interfaces in respective order for each prompt -

em0 -> WAN
em1 -> LAN
em2 -> Optional 1
em3 -> Optional 2
em4 -> Optional 3
em5 -> Optional 4
'Do you want to proceed [y|n]?' - y

pfsense Interface Selection

11. Select option 2. We will configure the LAN Interface so select 2 again.
We will use IP Address 192.168.1.1 to access the pfsense WebGUI. Configure the LAN Interface same as below.

pfsense LAN Interface

For, 'Do you want to revert to HTTP as the webConfigurator protocol? (y/n)' - n

12. The Configuration for OPT1 Interface is -
pfsense OPT1 Interface

13. The Configuration for OPT2 Interace is -
pfsense OPT1 Interface

14. The OPT3 Interface should be left without an IP as it is going to have the span port with traffic that the IDS (Security Onion) is going to be monitoring.

15. The Configuration for OPT3 Interface is -

pfsense OPT1 Interface

16. With this we have configured the pfsense VM. Rest of the configuration will be done using the Parrot Machine through the WebConfigurator.

# Installing and Configuring the IDS - Security Onion
