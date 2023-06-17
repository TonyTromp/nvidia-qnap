# Compiling the NVIDIA Drivers
```
# extract the NVIDIA Driver.run file
```
chmod a+x <NVIDIA_Driver>.run

./<NVIDIA_Driver>.run --extract-only
```

This will create a folder called <NVIDIA_Driver>
```
cd <NVIDIA_Driver>
```

# note i have compiled binutils and glibc2.1 in /share/Public/opt/ 
ln -s /share/Public/opt/include /usr/include
ln -s /share/Public/opt/bin/awk /usr/bin/awk
ln -s /share/Public/opt/include /usr/include

export PATH=/share/Public/opt/bin:/share/Public/qnap/x86_64-QNAP-linux-gnu/cross-tools/bin:${PATH}
export CC=gcc
export TERM=ansi
```

```
#
# fix error: kernel/nvidia/nv-caps.c:567:5: error: implicit declaration of function 'sys_close' [-Werror=implicit-function-declaration]
#     sys_close(fd);
#     ^
#
# EDIT kernel/nvidia/nv-caps.c
vi kernel/nvidia/nv-caps.c
# then add the following line 43 (on a empty line)
#define NV_IS_EXPORT_SYMBOL_PRESENT___close_fd 1

#export LD_LIBRARY_PATH="/lib:/lib64/:/usr/lib:/usr/local/lib:/share/Public/opt/lib:/share/Public/qnap/x86_64-QNAP-linux-gnu/fs/lib:/share/Public/openssl-1.1.1t:/share/Public/qnap/x86_64-QNAP-linux-gnu/fs/lib"

#
# fix error: ./tools/objtool/objtool: error while loading shared libraries: libelf.so.1: cannot open shared object file: No such file or directory
#
vi /etc/ld.so.conf
# add the following entries
/lib
/usr/lib
/usr/local/lib
/lib64
/share/Public/opt/lib
/share/Public/opt/lib64
# save and run
ldconfig

export KERNEL_SRC=/share/Public/qnap/GPL_QuTS_Hero/src/linux-5.10
export KERNEL_CFG=${KERNEL_SRC}/.config
export IGNORE_MISSING_MODULE_SYMVERS=1

# Create the build folder
mkdir build

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
--utility-prefix=${TARGET} \
--skip-depmod \
--no-check-for-alternate-installs \
--expert

#
#--kernel-module-build-directory=${TARGET} \

**NOTE: The build will fail to install, but dont worry, if all is well there should be goodies in your build folder**

```
**Note this listing is for the vGPU drivers**
cd build
ls -l

[/share/Public/nvidia/510.108.03-514.08/Host_Drivers/NVIDIA-Linux-x86_64-510.108.03-vgpu-kvm/build] # ls -l
total 38439
drwxr-xr-x 2 admin administrators        9 2023-06-10 02:48 bin/
drwxr-xr-x 2 admin administrators        9 2023-06-10 02:48 lib/
-rw-r--r-- 1 admin administrators 46044384 2023-06-10 02:48 nvidia.ko
-rw-r--r-- 1 admin administrators   104568 2023-06-10 02:48 nvidia-vgpu-vfio.ko
drwxr-xr-x 4 admin administrators        4 2023-06-10 02:48 share/
```

*note*: If you dont see the *.ko driver files, you will find them in the ./kernel folder. Just copy them (cp *.ko ../build) to the build folder.

Lets add the following script so the needed libraries can be easily loaded
# nvidia.env
```
#!/bin/sh

export LD_LIBRARY_PATH=${PWD}:${PWD}/build/lib:$LD_LIBRARY_PATH
export PATH=$PWD:$PWD/build/lib:$PATH
```

DONE!!!! Now to load the drivers
source nvidia.env
insmod <driver.ko>
# note that the order of which the NVIDIA Drivers need to be loaded.
