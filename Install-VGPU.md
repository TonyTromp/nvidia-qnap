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
```
# softlink dependencies
ln -s /share/Public/opt/include /usr/include
ln -s /share/Public/opt/bin/awk /usr/bin/awk

# set path to your build tools
export PATH=/share/Public/opt/bin:/share/Public/qnap/x86_64-QNAP-linux-gnu/cross-tools/bin:${PATH}

export LD_LIBRARY_PATH="/lib:/lib64/:/usr/lib:/usr/local/lib:/share/Public/opt/lib:/share/Public/qnap/x86_64-QNAP-linux-gnu/fs/lib:/share/Public/openssl-1.1.1t:/share/Public/qnap/x86_64-QNAP-linux-gnu/fs/lib"

export KERNEL_SRC=/share/Public/qnap/GPL_QuTS_Hero/src/linux-5.10
export KERNEL_CFG=${KERNEL_SRC}/.config

INSTALLER=./nvidia-installer 
TARGET=${PWD}/build
${INSTALLER} \
--no-distro-scripts \
--kernel-source-path=${KERNEL_SRC} \
--no-precompiled-interface \
--documentation-prefix=${TARGET} \
--installer-prefix=${TARGET} \
--utility-prefix=${TARGET} \
--kernel-install-path=${TARGET}  \
--opengl-prefix=${TARGET} \
--x-prefix=${TARGET} \
--x-library-path=${TARGET} \
--utility-prefix=${TARGET} 
```
