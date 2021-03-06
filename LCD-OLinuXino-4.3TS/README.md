# LCD-OLinuXino-4.3TS screen support

Olimex sells a cheap [4.3" touch screen](https://www.olimex.com/Products/OLinuXino/LCD/LCD-OLinuXino-4.3TS/open-source-hardware). They provides official images with that screen support, but they are based on [linux-sunxi](http://linux-sunxi.org). While more and more feature were implemented to the mainline Linux kernel, the linux-sunxi project became less attractive, and received less contributions.

There is few documentation to make work this screen with a mainline u-boot and linux kernel, but it's actually pretty easy. We need two parts:
* a u-boot configured to initialize the LCD display, with the good parameters (resolution, GPIO used for the connection)
* a kernel with simplefb support (available since 3.19, it's ok with Debian Stretch)

## Install cross-build utilities

If your main computer (not the Olimex A20) is running a debian, you just need to install crossbuild-essential-armhf :

		echo 'deb http://emdebian.org/tools/debian/ jessie main' >> /etc/apt/source.list
		curl http://emdebian.org/tools/debian/emdebian-toolchain-archive.key | sudo apt-key add -
		sudo dpkg --add-architecture armhf
		sudo aptitude update
		sudo aptitude install crossbuild-essential-armhf

## Compile and install u-boot with LCD-OLinuXino-4.3TS support

Get the u-boot sources, and checkout the v2015.04 commit:

		git clone http://git.denx.de/u-boot.git/
		cd u-boot
		git checkout -b 2015-07 33711bdd4a4dce942fb5ae85a68899a8357bdd94

Get the configuration for LCD-OLinuXino-4.3TS support:

		wget https://raw.githubusercontent.com/st3rk/Olimex_A20/master/LCD-OLinuXino-4.3TS%20screen%20support/A20-OLinuXino_MICRO-lcd4_defconfig -O configs/A20-OLinuXino_MICRO-lcd4_defconfig

Configure and compile uboot:

		make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- A20-OLinuXino_MICRO-lcd4_defconfig
		make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-

Install the new u-boot image on the micro-SD card (replace sdX by the good device):

		dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8

## Useful links

The [official Debian documentation](https://wiki.debian.org/InstallingDebianOn/Allwinner#u-boot-ahci-support).  
[A project](http://karme.de/prisirah/) of open-source hardware and free software digital photo frames, using Olimex devices.  
The official [Olimex script](https://github.com/OLIMEX/OLINUXINO/blob/master/SOFTWARE/A20/A20-build/change_display_olimex_A20.sh) to add a display, with interesting values (resolution, refresh frequency...).  
Linux-sunxi [wiki article](http://linux-sunxi.org/LCD) about LCD screens.
