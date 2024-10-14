# Day 1

## Boot Loaders
<pre>
- is a system utility that is installed in the hard disk boot sector( byte 0, sector 0 )
- MBR stands for Master Boot Record is where the boot loader application is installed
- it is 512 bytes
- boot loader application is the first application that runs after the BIOS POST (Power On Self Test completes )
- the boot loader application searches your hard disk, looking for Operating Systems installed on it
- in case, the boot loader finds more than one OS then it gives a menu for the user to choose which OS they want to boot into
- in otherwords, boot loader allows only OS to be active at point of time, though many OS is installed in the system
</pre>  

## Hypervisor Overview
<pre>
- Hypervisor a.k.a Virtualization
- virtualization technology allows us to run multiple OS in the same laptop/desktop/server
- i.e many OS can be active at the same time in the same machine
- there are 2 types of Hypervisors
  1. Type 1 - Bare Metal Hypervisor ( used in Workstations and Servers )
  2. Type 2 - used in laptops/desktops/workstations
- this type of virtualization is called Heavy-weight
  - the reason is,each virtual machine requires dedicated hardware resources
    - dedicated CPU cores
    - dedicated RAM
    - dedicated Storage ( HDD/SSD )
- the OS runs within the Virtual Machine(VM/Guest OS), hence each VM represents one OS
- the OS that runs within the VM is a fully functional OS
</pre>

#### Type 1 - Bare Metal Hypervisor
<pre>
- Virtual Machine can be created directly on the server without any Host OS
- Examples
  - VMWare vsphere/vcenter
</pre>

#### Type 2 Hypervisor
<pre>
- Examples
  - VMWare
    - Fusion ( supports Mac OS-X )
    - Workstation ( supports Linux and Windows )
  - Oracle VirtualBox
  - Parallels ( supports Mac OS-X )
  - Microsoft Hyper-V
  - KVM - supported in all Linux distibutions
</pre>

## Hypervisor High Level Architecture

## Container Engine Overview

## Container Runtime Overview

## Docker Overview

## Docker High Level Architecture
