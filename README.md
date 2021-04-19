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

### Examples

To download the Real-time Edge 3.0 release

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/nxp-real-time-edge-sw/yocto-real-time-edge.git -b real-time-edge-scarthgap -m real-time-edge-3.0.0.xml
$ repo sync
```

## Setup build project

```
$ MACHINE=<Machine> DISTRO=<Distro> source ./real-time-edge-setup-env.sh -b bld-<Name>
```

Machine:
- imx6ull14x14evk
- imx8dxlb0-lpddr4-evk
- imx8mm-lpddr4-evk
- imx8mp-lpddr4-evk
- imx91-11x11-lpddr4-evk
- imx91-9x9-lpddr4-qsb
- imx93evk
- imx93-9x9-lpddr4-qsb
- imx93-14x14-lpddr4x-evk
- imx95-19x19-lpddr5-evk
- imx95-15x15-lpddr4x-evk
- ls1028ardb
- ls1043ardb
- ls1046ardb
- lx2160ardb-rev2

Distro:
- nxp-real-time-edge – The regular image including Real-time Networking, Real-time System, and Industrial packages.
- nxp-real-time-edge-baremetal – The baremetal image (some boards do not support this distro).
- nxp-real-time-edge-emmc – The emmc boot image (some boards do not support this distro).
- nxp-real-time-edge-plc – The PLC image (some boards do not support this distro).

Name:
- identical string for the build project

### Examples

```
$ DISTRO=nxp-real-time-edge-baremetal MACHINE=imx8mp-lpddr4-evk source real-time-edge-setup-env.sh -b build-imx8mpevk-baremetal
```

```
$ DISTRO=nxp-real-time-edge MACHINE=imx8mp-lpddr4-evk source real-time-edge-setup-env.sh -b build-imx8mpevk-real-time-edge
```

## Build an image

```
$ bitbake <Image>
```

Image:
- nxp-image-real-time-edge: demo image for all supported machines.
- nxp-image-real-time-edge-plc: The macro image to support PLC.

### Examples

```
$ bitbake nxp-image-real-time-edge
```
