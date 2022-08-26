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
$ repo init -u https://github.com/real-time-edge-sw/yocto-real-time-edge.git -b <branch name> -m <release manifest>
$ repo sync
```

### Examples

To download the Real-time Edge 2.3 release

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/real-time-edge-sw/yocto-real-time-edge.git -b honister-rpmsg-rs485 -m real-time-edge-rpmsg-rs485.xml
```

## Setup build project

```
$ MACHINE=<Machine> DISTRO=<Distro> source ./real-time-edge-setup-env.sh -b bld-<Name>
```

Machine:
- imx6ull14x14evk
- imx8mm-lpddr4-evk
- imx8mp-lpddr4-evk
- ls1028ardb
- ls1012ardb
- ls1021atwr
- ls1021atsn
- ls1021aiot
- ls1043ardb
- ls1046ardb
- ls1046afrwy
- lx2160ardb-rev2

Distro:
- nxp-real-time-edge – The regular image including Real-time Networking, Real-time System, and Industrial packages.
- nxp-real-time-edge-baremetal – The baremetal image (some boards do not support this distro).
- nxp-real-time-edge-avb – Very similar to the regular image with additional support for AVB endpoint (supported only on imx6ull14x14evk, imx8mm-lpddr4-evk and imx8mp-lpddr4-evk).

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

### Examples

```
$ bitbake nxp-image-real-time-edge
```
