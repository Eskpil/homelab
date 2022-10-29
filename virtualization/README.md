# Virtualization

Most of my homelab is virtualized since I don't have the resources to
run everything on hardware and virtualization gives me a very logical
separation of all my services.

## Libvirt

All the virtual machines use kvm with libvirt as their hypervisor.
Libvirt is well documented and has a great public facing API for
managing the virtual machines or domains as libvirt call them. Libvirt
also handles the storage pools and volumes used by domains and also
exposes this over a public facing API.

It also has a command line utility called virsh which is fine, it can do
simple things but personally I find the need for a more abstracted API.
Which is why I am creating Salmon, a system for managing my virtual
machines.

## MicroVMS

I am also planning to run MicroVMS in my architecture and these would
also be managed by the Salmon system.
