
```
# note i have compiled binutils and glibc2.1 in /share/Public/opt/ 
ln -s /share/Public/opt/include /usr/include
ln -s /share/Public/opt/bin/awk /usr/bin/awk
ln -s /share/Public/opt/include /usr/include

export PATH=/share/Public/opt/bin:/share/Public/qnap/x86_64-QNAP-linux-gnu/cross-tools/bin:${PATH}

export KERNEL_SRC=/share/Public/qnap/GPL_QuTS_Hero/src/linux-5.10
export KERNEL_CFG=${KERNEL_SRC}/.config

#export LD_LIBRARY_PATH="/lib:/lib64/:/usr/lib:/usr/local/lib:/share/Public/opt/lib:/share/Public/qnap/x86_64-QNAP-linux-gnu/fs/lib:/share/Public/openssl-1.1.1t:/share/Public/qnap/x86_64-QNAP-linux-gnu/fs/lib"

#
# fix error: kernel/nvidia/nv-caps.c:567:5: error: implicit declaration of function 'sys_close' [-Werror=implicit-function-declaration]
#     sys_close(fd);
#     ^
#
# EDIT kernel/nvidia/nv-caps.c
# add the following line 43
#define NV_IS_EXPORT_SYMBOL_PRESENT___close_fd = 1


export IGNORE_MISSING_MODULE_SYMVERS=1
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
