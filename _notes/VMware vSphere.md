---
Title: VMware vSphere
---

# vSphere

**vSphere** is VMware's solution that combines the [[VMware ESXi]] hypervisor and the vCenter Server management platform (:5480) 

From the vSphere Client you manage BOTH the VDDC (Virtual Data Center - the ESXi hosts) and the VMs we've created.

`vpxd` - heartbeat daemon in vCenter Server <- 443 -- *vSphere Client*
    |
  443
    |
   V
`vpxa` - heartbeat agent ESXi host VMs  <- -> `hostd`  <- 443 --* VMware Host Client*


## vSphere Cluster

A group of ESXi hosts whose resorces are shared by VMs.

## vSphere vMotion

Feature that enables the migration of powered-on VMs from host to host without service interruption.

## vSphere HA

Cluster feature that protects against host hardware failures by restarting VMs on hosts that are running normally.

## vSphere DRS

Cluster feature that uses vSphere vMotion to place VMs on hosts and ensure that each VM receives the resources that it needs


# About Virtual Machines

A VM is a software representation of a physical computer and its components. The virtualization software converts the physical machine and its components into files.

VMs typically include an operating system, applications, [[VMware Tools]] and virtual resources, such as CPU and memory, network adapters, disks and controllers, parallel and serial ports.

VMs are:
- Easy to move or copy
- Independent of hardware because VMs are encapsulated in files
- Isolated from other VMs running on the same physical hardware
- Insulated from physical hardware changes

In a physical machine the OS is installed directly on the hardware and requires specific drivers to support specific hardware. If the hardware is upgraded, new device drivers are required -> testing hardware for incompatibility with a wide array of applications costs time and money. 

Virtualizing these systems saves on such costs because VMs are 100% software.

VMs are isolated from each other, even an administrator on a VM's guest operating system cannot breach this layer of isolation to access another VM. These users must explicitly be granted access by the ESXi system administrator.

# About the Software-Defined Data Center

In a software-defined data center (SDDC), all infrastructure is virtualized, and the control of the data center is automated by software. [[VMware vSphere|vSphere]] is the foundation of the SDDC.

All the resources (CPU, momory, disk, and network) of a SDDC are abstracted into files. This abstraction brings the benefits of virtualization at all levels of the infrastructure, independent of the physical hardware.

