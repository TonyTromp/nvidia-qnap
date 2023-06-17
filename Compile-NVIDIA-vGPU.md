

Building on QNAP using cross-compile tools ./NVIDIA-Linux-x86_64-525.85.07-vgpu-kvm.run
```
# Extract NVIDIA src package
chmod a+x ./NVIDIA-Linux-x86_64-525.85.07-vgpu-kvm.run
./NVIDIA-Linux-x86_64-525.85.07-vgpu-kvm.run --extract-only

cd NVIDIA-Linux-x86_64-525.85.07-vgpu-kvm/

# Compile helper programs and folders (replace /share/Public/opt with your preffered base-directory/prefix)
ln -s /share/Public/opt/include /usr/include
ln -s /share/Public/opt/bin/awk /usr/bin/awk
ln -s /share/Public/opt/include /usr/include

# NOT NEEDED 
# Edit ldconfig
#vi /etc/ld.so.conf
# EDIT
#ldconfig

export KERNEL_SRC=/share/Public/qnap/GPL_QuTS_Hero/src/linux-5.10
export KERNEL_CFG=${KERNEL_SRC}/.config
export IGNORE_MISSING_MODULE_SYMVERS=1
export TERM=ansi

# Add the Compiler to system PATH
export PATH=/share/Public/opt/bin:/share/Public/qnap/x86_64-QNAP-linux-gnu/cross-tools/bin:${PATH}
export CC=gcc

INSTALLER=./nvidia-installer
TARGET=${PWD}/build
mkdir $TARGET

${INSTALLER} --no-distro-scripts --kernel-source-path=${KERNEL_SRC} --no-precompiled-interface --documentation-prefix=${TARGET} --installer-prefix=${TARGET} --utility-prefix=${TARGET} --kernel-install-path=${TARGET}  --x-prefix=${TARGET} --x-library-path=${TARGET} --utility-prefix=${TARGET} --kernel-module-source-prefix=${TARGET} --x-prefix=${TARGET} --skip-depmod --no-systemd --no-nvidia-modprobe --skip-module-unload --expert --no-dkms --no-rpms --no-backup --no-x-check --no-nouveau-check --no-distro-scripts --no-check-for-alternate-installs -j6

```
