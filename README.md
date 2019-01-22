Buildroot for ESPRESSObin
=========================

This branch of buildroot is for ESPRESSObin board and it is based on
[buildroot][buildroot] tag [2018.11][2018.11].

We only provide the build steps and some notes. You can find documentation at
the mainline of [buildroot][buildroot].

Note
====

We use buildroot only in two ways, one for independent system mainly used for
software update and other one is for standard booting ramfs mainly used for
system reset. For more detial please refer to project documentation for
[espressobin][espressobin documentation].

ESPRESSObin ramfs
-----------------

The ramfs used in ESPRESSObin is generated at current branch, two part need to
be noted.

Defconfig: configs/espressobin\_ramfs\_defconfig
Overlay: board/chenhan/espressobin\_ramfs/

Defconfig file specify to use the latest release of binutils and gcc, and
generate xz compressed [CPIO][CPIO] archive. And this CPIO will be linked into
Linux kernel (We chose this approach as default) or loaded by bootloader.

Overlay directory containing some files that will copy into final rootfs, any
existed files will be replaced by files in overlay directory. In this overlay
directory, we use 'init' shell script to overwrite the default behavior of
entering busybox's user space, to mount an OverlayFS as a new rootfs, to
support system reset feature. Those files in the overlay directory are very
simple, we are not going to descript them here.

ESPRESSObin rescue
------------------

The rescue image used in ESPRESSObin is reserved for rescue the firmware or do
some system upgrade job. Still on going...

ESPRESSObin USB rescue
----------------------

The USB rescue image used in ESPRESSObin is to generate an image which running
on a USB stick, and this image is response for rebuild any thing on board
SD/eMMC card include **mke2fs** and **parted**. If a system is completely
destroyed and have no way to upload image from **ESPRESSObin rescue mode**.
This USB rescue will take care of it.

Defconfig: configs/espressobin\_usb\_defconfig
Overlay: board/chenhan/espressobin\_usb/

Same as ESPRESSObin ramfs, ESPRESSObin USB rescue also compile as [CPIO][CPIO]
file and you should append this [CPIO][CPIO] file when build Linux kernel.

**Note**: To build USB rescue image, you may need **texinfo** package
(required by target package **parted**) installed in your host machine before
build this image.

TODO
====

1. ESPRESSObin rescue part

******

*Copyright (C) 2018, Hunan ChenHan Information Technology Co., Ltd. All rights reserved.*

[buildroot]: https://github.com/buildroot/buildroot "buildroot"
[2018.11]: https://github.com/buildroot/buildroot/tree/2018.11

[espressobin documentation]: https://github.com/chenhaninformation/documentation/tree/master/espressobin

[CPIO]: https://www.kernel.org/doc/html/latest/admin-guide/initrd.html#compressed-cpio-images

