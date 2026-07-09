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

To download the Real-time Edge release

```
$ mkdir yocto-real-time-edge
$ cd yocto-real-time-edge
$ repo init -u https://github.com/nxp-real-time-edge-sw/yocto-real-time-edge.git -b real-time-edge-wrynose -m real-time-edge-3.5.0.xml
$ repo sync
```

## Setup build project

```
$ MACHINE=<Machine> DISTRO=<Distro> source ./real-time-edge-setup-env.sh -b bld-<Name>
```

Machine:

#### Consolidated Machines (Recommended)

Consolidated machines build a single image (rootfs + kernel + bootloader) that supports
multiple board variants. The primary board boots directly; variant boards can be converted
by swapping the bootloader. This saves build time and disk space.

| Consolidated Machine | Primary Board (rootfs + bootloader) | Variant Boards (bootloader-only swap) |
|---|---|---|
| `imx8mmevk` | imx8mm-lpddr4-evk | - |
| `imx8mpevk` | imx8mp-lpddr4-evk | imx8mp-lpddr4-frdm |
| `imx91evk` | imx91-11x11-lpddr4-evk | imx91-11x11-lpddr4-frdm, imx91-11x11-lpddr4-frdm-imx91s, imx91-9x9-lpddr4-qsb |
| `imx93evk` | imx93-11x11-lpddr4x-evk | imx93-14x14-lpddr4x-evk, imx93-9x9-lpddr4-qsb, imx93-11x11-lpddr4x-frdm |
| `imx943evk` | imx943-19x19-lpddr5-evk | imx943-19x19-lpddr4-evk, imx943-15x15-lpddr4-evk |
| `imx95evk` | imx95-19x19-lpddr5-evk | imx95-19x19-lpddr5-frdm-pro, imx95-15x15-lpddr4x-evk, imx95-15x15-lpddr4x-frdm |
| `imx952evk` | imx952-19x19-lpddr5-evk | - |

#### Standalone Machines

These machines do not have a consolidated variant and must be built individually:

- imx6ull14x14evk
- imx8dxlb0-lpddr4-evk
- ls1028ardb
- ls1043ardb
- ls1046ardb
- lx2160ardb-rev2

#### Dedicated Machines (per-board build)

If you only need a single specific board, you can still build with dedicated machine configs
directly. These are the individual board variants that can also be built standalone:

**i.MX 8M Series:**
- imx8mm-lpddr4-evk
- imx8mp-lpddr4-evk
- imx8mp-lpddr4-frdm

**i.MX 91 Series:**
- imx91-11x11-lpddr4-evk
- imx91-11x11-lpddr4-frdm
- imx91-11x11-lpddr4-frdm-imx91s
- imx91-9x9-lpddr4-qsb

**i.MX 93 Series:**
- imx93-9x9-lpddr4-qsb
- imx93-11x11-lpddr4x-frdm
- imx93-14x14-lpddr4x-evk

**i.MX 943 Series:**
- imx943-19x19-lpddr4-evk
- imx943-19x19-lpddr5-evk
- imx943-15x15-lpddr4-evk

**i.MX 95 Series:**
- imx95-19x19-lpddr5-evk
- imx95-19x19-lpddr5-frdm-pro
- imx95-15x15-lpddr4x-evk
- imx95-15x15-lpddr4x-frdm

**i.MX 952 Series:**
- imx952-19x19-lpddr5-evk

Distro:
- nxp-real-time-edge – The regular image including Real-time Networking, Real-time System, and Industrial packages.
- nxp-real-time-edge-baremetal – The baremetal image (some boards do not support this distro).
- nxp-real-time-edge-emmc – The emmc boot image (some boards do not support this distro).
- nxp-real-time-edge-plc – The PLC image (some boards do not support this distro).

Name:
- identical string for the build project

### Examples

```
# Consolidated build (covers imx8mp-lpddr4-evk + imx8mp-lpddr4-frdm)
$ DISTRO=nxp-real-time-edge MACHINE=imx8mpevk source real-time-edge-setup-env.sh -b build-imx8mpevk-real-time-edge

# Dedicated build (single board only)
$ DISTRO=nxp-real-time-edge MACHINE=imx8mp-lpddr4-evk source real-time-edge-setup-env.sh -b build-imx8mpevk-real-time-edge

# LayerScape build
$ DISTRO=nxp-real-time-edge MACHINE=ls1028ardb source real-time-edge-setup-env.sh -b build-ls1028ardb-real-time-edge
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
# Build full image (rootfs + kernel + bootloader)
$ bitbake nxp-image-real-time-edge
```

## Build bootloader only

For variant boards that share the rootfs with a consolidated machine, you only need to build
the bootloader. Setup the build environment with the variant board's MACHINE, then build `imx-boot`.

### Examples

```
# Build bootloader for a variant board (e.g. imx95-15x15-lpddr4x-frdm)
$ DISTRO=nxp-real-time-edge MACHINE=imx95-15x15-lpddr4x-frdm source real-time-edge-setup-env.sh -b bld-imx95-frdm
$ bitbake imx-boot
```

## Boot image variants (IMXBOOT_VARIANT)

For i.MX 95/943/952, the bootloader can be built with different firmware variants by setting
`IMXBOOT_VARIANT` in `conf/local.conf`. This selects a different System Manager configuration
and boot target.

| IMXBOOT_VARIANT | Description | Supported SoCs |
|---|---|---|
| *(empty)* | Default boot (uses RTE System Manager config) | All |
| `netc` | Network Controller enabled boot | i.MX 95, 943, 952 |
| `netc_reset` | Network Controller with reset support | i.MX 943 |
| `netc_standalone` | Network Controller standalone mode | i.MX 943 |

### Examples

```
# Build bootloader with NETC variant for i.MX 95
$ echo 'IMXBOOT_VARIANT = "netc"' >> conf/local.conf
$ bitbake imx-boot
```
