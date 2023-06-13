# Pre-requisites

## Loading the drivers
If you have not done so, load the necessary Kernel + Nvidia drivers to enable vGPU.

```
# From previous steps (see Install vGPU) load the following modules in order
insmod ./mdev.ko
insmod ./vfio_mdev.ko
insmod ./nvidia.ko
insmod ./nvidia-vgpu-vfio.ko
```

After the drivers are loaded, the vGPU is not yet enable, and you need to run the command
```
# nvidia source
cd <nvidia_source>
export LD_LIBRARY_PATH=${PWD}/bin:${PWD}/lib:$LD_LIBRARY_PATH
export PATH=$PWD/bin:$PWD/lib:$PATH

# note if build/bin folder does not exists, please follow the steps mentioned [Compile-NVIDIA-Drivers.md](Compile-NVIDIA-Drivers.md).
cd build/bin
nvidia-vgpud
```



With the the vGPU device created using the 'mdevctl' command on Ubuntu Station, you should be able to validate using the following command and outputt.

```
dmesg

[ 4001.093277] nvidia 0000:01:00.0: MDEV: Registered
[ 4001.307790] vfio_mdev 35dfcced-44f9-40cd-993b-a296aacfb83a: Adding to iommu group 26
[ 4001.307792] vfio_mdev 35dfcced-44f9-40cd-993b-a296aacfb83a: MDEV: group_id = 26
```

Now we are ready to update Container Station.
Note that this step you need to re-do every time you reboot Container Station as QNAP resets these settings.

First we need to set some variables so we edit the config.
```
export PATH=/QVS/usr/bin:/QVS/usr/sbin:$PATH
```

now we can run the ```virsh``` command and edit our VM to use the vGPU device we have created.
```
virsh

virsh # list --all
 Id   Name                                   State
-------------------------------------------------------
 -    0458c1a9-f562-4728-b424-64284b09fc61   shut off
 -    476b932f-bee5-400a-b4dc-2e5a3e5961a0   shut off
 -    cbd3ef58-2407-47e7-a16d-2e65072bab62   shut off
 -    d137b7f6-594b-4703-a4c7-8611cb56e9cb   shut off
```

this should list all your configured VMs in Container Station. If this is empty, then first setup a VM using Container Station.
You can use:
```
edit <uuid>
```
To edit a specific VM configuration. As they only list with uuid, i suggest you keep track which uuid corresponds to which VM (use the edit feature to look at the contents).

```
edit <uuid-you-want to edit>
# Look for the following content (note the basic Vim commands 'i' to insert, and :wq to save).
  <devices>
  ...
```

After the line ```<devices>``` insert the following contteen.

```
<hostdev mode='subsystem' type='mdev' managed='no' model='vfio-pci'>
  <source>
    <address uuid='35dfcced-44f9-40cd-993b-a296aacfb83a'/>
  </source>
</hostdev>
```

Replace the UUID with the UUID of your vGPU.






