---
title: How to create a Windows 2012 Virtual Machine
category: howto
authors: nkesick
wiki_title: How to create a Windows 2012 Virtual Machine
wiki_revision_count: 4
wiki_last_updated: 2014-01-01
---

# How to create a Windows 2012 Virtual Machine

## Introduction

In your current configuration, you should have at least one host available for running virtual machines, and uploaded the required installation images to your ISO domain. This section guides you through the creation of a Windows 2012 virtual machine. You will perform a normal attended installation using a virtual DVD.

### VirtIO interfaces

**Please read this section** before jumping to the installation part. If you are familiar with Linux VMs on oVirt you know that the default options work fairly well. This is not the case with Windows.

*   For Disks there are three interface options - VirtIO, VirtIO-SCSI, and IDE. **VirtIO** (default) is the recommended interface but it requires additional drivers to be present at install and after the installation, much like servers or desktops with RAID and SCSI interfaces. **IDE** is an optional alternative that does not require the additional drivers but may show some performance issues.
*   For Networking there are three interface options - VirtIO, e1000, and rtl8139. **VirtIO** (default) is the recommended interface but it requires additional drivers to be present after the installation which is a common issue for Windows desktops and servers after reinstalling the OS. **e1000** and **rtl8139** are optional alternatives that do not require the additional drivers (depending on the Windows OS) but may show some performance issues. The network interface can be changed after installing.

Loading the VirtIO drivers and using the alternatives is covered in the install directions below. If you would like to use the VirtIO interfaces you only need to add the VirtIO disk to your ISO domain. [Please see this section to download the VirtIO ISO from Fedora](/documentation/internal/guest-agent/understanding-guest-agents-and-other-tools/#virtio-drivers) which contains signed drivers for Windows.

## Creating a Windows 2012 VM

1. From the navigation tabs, select Virtual Machines. On the Virtual Machines tab, click New VM.

![](Navigation_Tabs.jpg "Navigation_Tabs.jpg")

Figure 2.1: The navigation tabs

2. The “New Virtual Machine” popup appears.

![](New_VM_Win2012.jpg "New_VM_Win2012.jpg")

Figure 2.2: Create new Windows virtual machine

3. Under General, your default Cluster and Template will be fine.

4. For Operating System, choose Windows 2012 x64.

5. Under Optimized For, choose Server.

6. Add a Name (required) and a comment or description (optional).

7. Finally, attach a Network Interface (optional) to the VM by selecting one from the dropdown.

8. Click OK

      Note: By clicking “Additional Options” you can configure other details such as memory and CPU resources. You can change these after creating a VM as well, 

9. A New Virtual Machine - Guide Me window opens. This allows you to add storage disks to the virtual machine.

![](Guide_Me.jpg "Guide_Me.jpg")

Figure 2.3. New Virtual Machine – Guide Me

11. Click Configure Virtual Disks to add storage to the virtual machine.

12. Enter a Size for the disk.

13. Click OK

      The parameters in the following figure such as Interface and Allocation Policy are recommended, but can be edited as necessary. 

![](Add_Virtual_Disk_Win2012.jpg "Add_Virtual_Disk_Win2012.jpg")

Figure 2.4. Add Virtual Disk configurations

      Note: `[`As` `mentioned` `above`](How_to_create_a_Windows_2012_Virtual_Machine#VirtIO_interfaces)` When using the VirtIO interface (recommended) additional drivers are required at install time. You can use the IDE interface instead which does not require the additional drivers. The OS install guide covers both VirtIO and IDE interfaces below.

15. Close the Guide Me window by clicking Configure Later. Your new Windows 2012 virtual machine will display in the Virtual Machines tab.

You have now created your Windows 2012 virtual machine. Before you can use your virtual machine you need to install an operating system on it.

## Installing an Operating System

1. Right click the virtual machine and select Run Once.

2. Check “Attach CD” and choose a disk from the list

      Note: If you do not have any in the list, you need to upload one.

3. Click Ok

![](Run_Once_Win2012.jpg "Run_Once_Win2012.jpg")

Figure 3.1. Run once menu

      Retain the default settings for the other options and click OK to start the virtual machine. 

4. Select the virtual machine and click the Console ( ) icon. This displays a window to the virtual machine, where you will be prompted to begin installing the operating system.

5. Continue with the Windows 2012 install as normal until you reach "Where do you want to install Windows?"

### Installing with a VirtIO interface

<div class="toccolours mw-collapsible mw-collapsed" style="width:800px">
"Where do you want to install Windows?" does not show any disks. Click to expand this section.

<div class="mw-collapsible-content">
![No disks available](Install_Windows2012_VirtIO_Disk.jpg "fig:No disks available") You need to load the VirtIO driver.

1. On the Navigation Tabs, click Change CD![Change CD](Navigation_Tabs_Change_CD.jpg "fig:Change CD")

2. From the drop down list select the virtio CD and click ok.![VirtIO CD](Change CD virtio.jpg "fig:VirtIO CD")

3. On the console, click "Load Drivers"

4. On the "Load Driver" popup, click Browse

5. Browse to the CD, Win8 folder folder. Choose the appropriate architecture (AMD64 for 64-bit) and click OK.

6. The VirtIO Drivers should appear. Choose "Red Hat VirtIO SCSI Controller", and then click Next![Drivers Available](Install_Windows2012_VirtIO_Drivers.jpg "fig:Drivers Available")

7. The driver should install and return to the "Where do you want to install Windows?" screen now showing a disk to install to. Note that a message has appeared that "Windows cannot be installed to this disk"

8. On the Navigation Tabs, click Change CD

9. From the drop down list select the Windows 2012 install media and click ok.

10. On the console, click "Refresh". The "Windows cannot be installed to this disk" message should disappear as the system can see the Windows install media again.

11. Continue with the install as normal

</div>
</div>
### Installing with a IDE interface

"Where do you want to install Windows?" shows a disk to install to. Continue as normal.

## Post Install Additions

### Drivers

#### VirtIO

If you wish to use the oVirt Guest Tools through the VirtIO-Serial interface, the VirtIO network interface, or a SCSI disk you need to install additional drivers. ![Device Manager](Device_Manager_Win2012_Missing_Drivers_VirtIO.jpg "fig:Device Manager")

1.  On the console, open the Device Manger
2.  On the Navigation Tabs, click Change CD![Change CD](Navigation_Tabs_Change_CD.jpg "fig:Change CD")
3.  From the drop down list select the virtio CD and click ok.![VirtIO CD](Change CD virtio.jpg "fig:VirtIO CD")

##### VirtIO Serial

1.  On the console, right click the **PCI Simple Communications Controller** device that is missing drivers
2.  Select "Update Driver", and then click Next
3.  Choose "Browse my computer for driver software"
4.  Browse to the CD, Win8 folder folder. Choose the appropriate architecture (AMD64 for 64-bit) and click OK., and then Next.
5.  If prompted, choose "Install this driver software anyway"
6.  When prompted, choose "Always trust software from Red Hat Inc.", and click Install.

##### VirtIO Networking

1.  On the console, right click the **Ethernet Controller** device that is missing drivers
2.  Select "Update Driver", and then click Next
3.  Choose "Browse my computer for driver software"
4.  Browse to the CD, Win8 folder. Choose the appropriate architecture (AMD64 for 64-bit) and click OK., and then Next.
5.  If prompted, choose "Install this driver software anyway"
6.  When prompted, choose "Always trust software from Red Hat Inc.", and click Install.

##### VirtIO SCSI

1.  On the console, right click the **SCSI Controller** device that is missing drivers
2.  Select "Update Driver", and then click Next
3.  Choose "Browse my computer for driver software"
4.  Browse to the CD, Win8 folder. Choose the appropriate architecture (AMD64 for 64-bit) and click OK., and then Next.
5.  If prompted, choose "Install this driver software anyway"
6.  When prompted, choose "Always trust software from Red Hat Inc.", and click Install.

### Guest Tools

Adding a few guest tools may improve your experience.

*   oVirt Guest Agent allows oVirt to show the Memory and Network utilization of the VM, the IP address of the VM, the installed Applications, Enable Single Sign On (SSO) and more.
*   Spice-vdagent allows for copy and paste support (text & image), better mouse functionality, and automatic adjustment of the screen resolution based on the size of your window.

Add the oVirt Guest Agent by following the directions at <<unwritten>> Add the Spice-vdagent by following the directions at <<unwritten>>
