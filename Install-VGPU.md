(TODO) Full guide

# Pre-requisites
The following need to be setup in order the create a working vGPU environment
1. Setup QNAP ToolChain + QNAP Kerner Headers
2. Compile kernel modules "mdev.ko" and "vfio_mdev.ko" <- necessary for the vGPU Drivers
3. 

## 1. Setup QNAP ToolChain + QNAP Kerner Headers
- Setup a working kernel build environment using the QNAP Kernel Headers (https://sourceforge.net/projects/qosgpl/files/) and the Tool Chain (https://sourceforge.net/projects/qosgpl/files/QNAP%20NAS%20Tool%20Chains/)
- Extract your source files to target folder
```# eg /share/Public/qnap/GPL_QuTS_Hero````
- Copy the right kernel config file for your QNAP Device
```
# Note that for your device this path might be different
cp /share/Public/qnap/GPL_QuTS_Hero/kernel_cfg/TS-X72/linux-5.10-x86_64.config \
 /share/Public/qnap/GPL_QuTS_Hero/src/linux-5.10/.config
```

