## Disclaimer
This guide is not yet comlete, as i still need to backtrace lots of my steps for a final write-up.
I am not responsible in any way if you end-up breaking your system because of this guide.

# 1. Pre-requisites (TODO)
The following need to be setup in order the create a working vGPU environment
1. Setup QNAP ToolChain + QNAP Kerner Headers
2. Compile kernel modules "mdev.ko" and "vfio_mdev.ko" <- necessary for the vGPU Drivers
3. Enable/Load "mdev.ko" and "vfio_mdev.ko"
```
insmod mdev.ko
insmod vfio_mdev.ko
```

## 1. Setup QNAP ToolChain + QNAP Kerner Headers (TODO)
- Setup a working kernel build environment using the QNAP Kernel Headers (https://sourceforge.net/projects/qosgpl/files/) and the Tool Chain (https://sourceforge.net/projects/qosgpl/files/QNAP%20NAS%20Tool%20Chains/)
- Extract your source files to target folder
```# eg /share/Public/qnap/GPL_QuTS_Hero````
- Copy the right kernel config file for your QNAP Device
```
# Note that for your device this path might be different
cp /share/Public/qnap/GPL_QuTS_Hero/kernel_cfg/TS-X72/linux-5.10-x86_64.config \
 /share/Public/qnap/GPL_QuTS_Hero/src/linux-5.10/.config
```

## 2. Compile NVIDIA Drivers on QNAP

> [Compiling NVIDIA Drivers](./Compile-NVIDIA-Drivers.md)

NOTE: The build will fail, however if all goes well the drivers are already compiled.
You can find the drivers in ```<nvidia_driver>/kernel``` folder.

The files we care about are:

* nvidia.ko
* nvidia-vgpu-vfio.ko

## 3. Loading the drivers

Make sure the following order of loading drivers

1. mdev.ko
2. vfio_mdev.ko
3. nvidia.ko
4. nvidia-vgpu-vfio.ko

```
# If you copy all the *.ko files in one folder you load them using
insmod ./mdev.ko
insmod ./vfio_mdev.ko
insmod ./nvidia.ko
insmod ./nvidia-vgpu-vfio.ko
```

Then load the variables using the following command
```
source nvidia.env
```

run the following commands to enable MDEV and vGPU drivers
```
sudo ./nvidia-vgpud
```

check if driver is loaded
```
dmesg
# look for the following line at the end
[ 4001.093277] nvidia 0000:01:00.0: MDEV: Registered
```

IF THIS DOES NOT WORK.. a trick is to copy the ./nvidia-vgpud binary to ubuntu station. Open a terminal and run the file from here using sudo:
```
sudo ./nvidia-vgpud
```

To manage your vGPU instances, you can install mdevctl on Ubuntu Station:
```
apt-get install mdevctl
```

