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


## Server Grade Processor
- it supports 128,256,512 cpu cores
- Motherboards with 8 Processor Sockets
- Processors also come in 2 types of packaging
  - SCM ( single chip module )
  - MCM ( multiple chip module )


## Minimal number of Physical server to support 1000 Virtual Machines
- assume the server motherboards supports 8 Processor Sockets
- if we install MCM based Processor, i.e each IC has 4 Processor, each Processor supporting 256 cores
- total number of Physical CPU cores - 8 x 4 x 256 = 8192
- total logical/virtual CPU cores - 8192 x 2 = 16384



## Hypervisor High Level Architecture


## Linux Kernel that supports containerization
<pre>
1. Namespace - isolating one container from other containers
2. Control Groups ( CGroups ) 
   - helps in applying resource quoto restrictions to individual containers
   - we can restrict,how much maximum RAM a container can utilize 
   - we can restrict, how many CPU cores a container can utilize
</pre>  

## Containerization
<pre>
- is an application virtualization technology
- light-weight virtualization
  - each containized application doesn't require dedicated hardware resources
  - all containers that runs in a machine shares the Hardware resources available in the underlying OS
- each container represents a single application
- each container get an IP address
- each container get its own file system
- container is an application process that runs in a separate namespace
- containers are not a replacement for Virtualization or OS
- containers and virtualization are complementing technology, hence they are used in combination in real world
- each container get its own dedicated network namespace
- each container get its own dedicated network stack ( 7 OSI Layers )
- each container get its own dedicated port range ( 0 - 65535 )
- most of the containers has atleast one network card (virtual network card)
</pre>

## Container Engine Overview
<pre>
- is a high-level software that helps u managing container images and containers
- user-friendly 
- under the hood, container engines depends on Container runtimes to manage images and containers
- Examples
  - Docker is a Container Engine that depends on containerd which in turn depends on runC container runtime
  - Podman is a Container Engine that depends on CRI-O container runtime
</pre>

## Container Runtime Overview
<pre>
- is low-level software utility that helps us managing container images and containers  
- they are not user-friendly, hence normal end-users avoid using them directly
- examples
  - runC container runtime
  - CRI-O container runtime
</pre>


## Docker Overview

## Docker High Level Architecture
![Docker](DockerHighLevelArchitecture.png)
