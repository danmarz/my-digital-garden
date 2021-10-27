---
Title: ESXi
---

VMware ESXi (formerly ESX) is an enterprise-class, type-1, **bare-metal** hypervisor developed by VMware for deploying and serving virtual computers.

ESXi runs on bare metal (without running an operating system) unlike other VMware products. It includes it's own kernel.

The vmkernel is a microkernel with three interfaces: hardware, guest systems and the service console.

High security:
- Host-based firewall
- Memory hardening
- Kernel module integrity
- Trusted Platform Module (TPM 2.0)
- UEFI secure boot
- Encrypted core dumps
	- Small disk footprint
	- Quick boot for faster patching and upgrades
	- Installable on hard disks, SAN LUNs, SSD, USB devices, SD cards, SATADOM, and diskless hosts

## Physical File Systems and Datastores

[[VMware vSphere]] VMFS provides a distributed storage architecture, where multiple [[VMware ESXi|ESXi]] hosts can read or write to the shared storage concurrently.

To store virtual disks, [[VMware ESXi]] uses *datastores*, which are logical containers that hide the specifics of physica storage from VMs and provide an uniform model for storing VM files. Datastores that you deploy on block storage devices uses the **VMFS** format, a high-performance file system that is optimized for storing virtual machines.

## Configuring an ESXi host

The DCUI is a text-based user interface with keyboard-only interaction. 

You use the *Direct Console User Interface* (DCUI) to configure certain settings for ESXi hosts. The DCUI is a low-level configuration and management interface, accessible through the console of the server, that is used primarily for initial basic configuration. You press F2 to start customizing system settings.

# About Kubernetes (k8s)

Kubernetes manages containers across multiple container hosts, similar to how vCenter Server manages all ESXi hosts in a cluster. Running Docker without Kubernetes is like running ESXi hosts without vCenter Server to manage them.
