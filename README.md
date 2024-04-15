# Real-time Edge Software Manifest README

This repo is used to download manifests for Real-time Edge Software releases.

Specific instructions will reside in READMEs in each branch.

## Install the `repo` utility (only need to do this once)

To get the Real-time Edge environment you need to have `repo` installed.

This 'repo' is used to download manifests for Real-time Edge releases.

```
$ mkdir ~/bin
$ curl https://storage.googleapis.com/git-repo-downloads/repo  > ~/bin/repo
$ chmod a+x ~/bin/repo
$ export PATH=${PATH}:~/bin
```

## Download the specific Real-time Edge Environment

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/nxp-real-time-edge-sw/yocto-real-time-edge.git -b <branch name> -m <release manifest>
$ repo sync
```
> *NOTE*: If you get error on repo init - it may be due to python version. Try
https://community.nxp.com/t5/i-MX-Processors-Knowledge-Base/repo-issue-quot-def-print-self-args-kwargs-quot/ta-p/1756521



### Examples

To download the Real-time Edge 2.5 release + LA9310 Support

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/nxp-real-time-edge-sw/yocto-real-time-edge.git -b la93xx -m rte_la93xx_release.xml
	(Please use -m rte_la93xx_devel.xml for NXP internal builds to use development repos) 
```

## Setup build project

```
$ MACHINE=<Machine> DISTRO=<Distro> source ./real-time-edge-setup-env.sh -b bld-<Name>
```

Machine:
- imx8mp-rfnm
- imx8dxlb0-lpddr4-evk
- imx8mm-lpddr4-evk
- imx8mp-lpddr4-evk

Distro:
- nxp-real-time-edge – The regular image including Real-time Networking, Real-time System, and Industrial packages.

Name:
- identical string for the build project

### Examples


```
$ DISTRO=nxp-real-time-edge MACHINE=imx8mp-rfnm source real-time-edge-setup-env.sh -b build-imx8mprfnm
```

## Build an image

```
$ bitbake <Image>
```

Image:
- nxp-image-real-time-edge: demo image for all supported machines.

### Examples

```
$ bitbake nxp-image-real-time-edge
```

 
> Note: Sample commands for building individual components in this framework
```
bitbake -c clean freertos-la9310
bitbake -c do_compile -f freertos-la9310
bitbake -c do_compile -f kernel-module-la9310
bitbake -c do_compile -f userapp-la9310
```


## Generated Image Location

Below is the generated images location:
```
build-imx8mprfnm/tmp/deploy/images/imx8mp-rfnm$ ls -a *.wic*
 nxp-image-real-time-edge-imx8mp-rfnm-20240202081724.rootfs.wic.zst
 nxp-image-real-time-edge-imx8mp-rfnm.wic.zst -> nxp-image-real-time-edge-imx8mp-rfnm-20240202081724.rootfs.wic.zst
```

## Unzip Image and Flash in SD Card 

> (Make Sure correct device is being used w.r.t SD card e.g. /dev/sdX)
```
zstd -d *wic.zst
sudo dd if=nxp-image-real-time-edge-imx8mp-rfnm-20240202081724.rootfs.wic of=/dev/sdX bs=8M oflag=direct status=progress
```
 
## How To Load Shiva Kernel Module at Linux Prompt

```
echo "1" > /sys/bus/pci/rescan
echo 7 > /proc/sys/kernel/printk
    < additional steps for RFNM board > 
    gpioget 2 9
	gpioget 2 14
	gpioset 0 1=1
insmod /lib/modules/5.15.71-rt51+g8d6bc216a295/extra/la9310shiva.ko scratch_buf_size=0x4000000 scratch_buf_phys_addr=0x92400000
```

## Standalone component build - when using pre-built images

### Repo and Branches

| Component      | Github Location                              | Branch                |
|----------------|----------------------------------------------|-----------------------|
| Linux          | git://github.com/nxp-qoriq/linux.git         |`la12xx-linux-5.15-rt` |
| LA9310_HOST    | git://github.com/nxp-qoriq/la93xx_host_sw    |`main`                 |
| LA9310_FRTOS   | git://github.com/nxp-qoriq/la93xx_freertos   |`main`                 |
| LA9310_FW      | git://github.com/nxp-qoriq/la93xx_firmware   |`main`                 |


### How To Build Linux
> *Note*: RFNM kernel patches are not available in git://github.com/nxp-qoriq/linux.git branch: la12xx-linux-5.15-rt
	Please apply  kernel support patches manually to this tree: (https://github.com/nxp-real-time-edge-sw/meta-real-time-edge/tree/la93xx/dynamic-layers/imx-layer/recipes-kernel/linux/linux-imx)
```
export CROSS_COMPILE=<compiler-path>aarch64-linux-gnu-
export ARCH=arm64
make imx8mp_rfnm_defconfig
make -j 8
```
 
 ### How To Build FreeRTOS

```
export CROSS_COMPILE=<compiler-path>aarch64-linux-gnu-
export ARCH=arm64
export KERNEL_DIR=<kernel-path>/imx8mp-kernel
export ARMGCC_DIR=<toolchain-path>/gcc-arm-none-eabi-6-2017-q2-update/      
export LA9310_COMMON_HEADERS=<repository-path>/la931x_freertos/common_headers
cd ~/la931x_freertos/Demo/CORTEX_M4_NXP_LA9310_GCC/
./clean.sh
./build_release.sh -m pcie -t rfnm -b Release
```
 

### How to Build HOST

```
export CROSS_COMPILE=<compiler-path>aarch64-linux-gnu-
export ARCH=arm64
export KERNEL_DIR=<kernel-path>/imx8mp-kernel    
export LA9310_COMMON_HEADERS=<repository-path>/la931x_freertos/common_headers
make clean
make
make install
```
