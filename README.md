# Build Linux for Eclypse-Z7

## Environment

- Vivado 2019.1.3

## Build U-boot

```
$ git clone git://git.denx.de/u-boot.git
$ cd u-boot
$ git checkout -b v2017.11 refs/tags/v2017.11
$ patch -p1 < ../resources/u-boot.diff
$ make ARCH=arm zynq_eclypsez7_defconfig
$ make -j8 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- all
```

## Build Linux

```
$ git clone https://github.com/Xilinx/linux-xlnx.git
$ cd linux-xlnx
$ git checkout -b linux-xlnx-v2019.1-eclypsez7 refs/tags/xilinx-v2019.1
$ patch -p1 < ../resources/linux-xlnx.diff
$ git add arch/arm/boot/dts/zynq-eclypsez7.dts
$ git add --update
$ git commit -m "for Eclypse-Z7"
$ git tag -a xilinx-v2019.1-eclypsez7-3 -m "release xilinx-v2019.1-eclypsez7-3"
$ echo 3 > .version
$ make xilinx_zynq_defconfig ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
$ patch -p0 < ../resources/dot.config.patch
$ make -j32 deb-pkg ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- DTC_FLAGS=--symbols
$ make uImage ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- UIMAGE_LOADADDR=0x8000
```
