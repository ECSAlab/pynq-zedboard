# pynq-zedboard
Steps to reproduce the build of PYNQ on ZedBoard

## Prerequisites:
1) Ubuntu 18.04.4 LTS (https://old-releases.ubuntu.com/releases/18.04.4/)vnet-digilent-zedboard-v2020.1-final.bsp
2) Vitis 2020.1 (https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis/archive-vitis.html)
3) Petalinux 2020.1 (https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools/archive.html)
4) avnet-digilent-zedboard-v2020.1-final.bsp (https://www.xilinx.com/member/forms/download/xef.html?filename=avnet-digilent-zedboard-v2020.1-final.bsp)
5) pynq_z1_v2.6.0.img (http://bit.ly/pynqz1_v2_6)

## Preparation:
```shell=
$ cd Xilinx/PYNQ/boards
$ mkdir zedboard
$ cp Pynq-Z1/base zedboard/
$ cd zedboard
```
--> bring .bsp and .img here

--> generate "zedboard.spec" with the following content:

```console=
######################################

ARCH_zedboard := arm
BSP_zedboard := avnet-digilent-zedboard-v2020.1-final.bsp
FPGA_MANAGER_Pynq-Z1 := 0
STAGE4_PACKAGES_zedboard := boot_leds ethernet pynq jupyter pandas uart

#######################################
```

--> move other board folders from "Xilinx/PYNQ/boards" to reduce compilation time

## Compilation:
```shell=
$ source /tools/Xilinx/Vitis/2020.1/settings64.sh
$ source ../../petalinux/2020.1/settings.sh
$ petalinux-util --webtalk off
$ cd Xilinx/PYNQ/sdbuild/
$ make delete
$ make unmount
$ make clean
$ make BOARDDOR=../boards/zedboard/ PREBUILD=../boards/zedboard/pynq_z1_v2.6.0.img
```

## SDcard preparation:
1) Copy the contents of "Xilinx/PYNQ/sdbuild/output/boot/zedboard/" to the SD card.
2) Boot the Zedboard and find the IP address that it is acquired through DHCP and open a browser and connect with "http://ip_address"
