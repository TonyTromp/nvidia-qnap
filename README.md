# nvidia-qnap-vGPU and other usefull stuff for Container Station.

## Background

As i have sacrificed the last two months on getting my NVidia (Tesla P4) card to play nice with my QNAP, i hope these articles can provide guidance for those who need it.

In this Repository you will find usefull information on how to compile and use NVIDIA drivers on a non-modified QNAP system.
You will be able to compile the latest drivers yourself, enable VFIO/PCI passthrough (GPU Passthrough) for most NVidia cards (non-officially supported by QNAP) and finally split your card virtually, so it can be used in multiple VM's.

# How to setup VFIO/PCI Passthrough
* How to enable VFIO drivers and disable NVIDIA drivers
* How to modify your VM using virsh and enable passthrough
* Profit

# How to setup vGPU
* How to compile NVidia vGPU drivers on QNAP host
* How to set-up and run a VM using NVidia vGPU QNAP drivers
* Profit
