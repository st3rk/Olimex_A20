# Debian installation

## Create the bootable sd-card

The Allwinner A20 support is still a work in progress in the mainline Linux kernel. Therefore, I chose a Debian stretch to get recent packages.

First, download the stretch image:

		cd ~
		mkdir A20_Debian_image
		cd A20_Debian_image
		wget http://ftp.uk.debian.org/debian/dists/stretch/main/installer-armhf/current/images/netboot/SD-card-images/firmware.A20-OLinuXino-MICRO.img.gz
		wget http://ftp.uk.debian.org/debian/dists/stretch/main/installer-armhf/current/images/netboot/SD-card-images/partition.img.gz

Then, as root, write it on a micro-SD card

    # replace sdX by the SD-card device name
    zcat firmware.A20-OLinuXino-MICRO.img.gz partition.img.gz > /dev/sdX

## Install debian on the Olimex A20 Micro

Just insert the micro-SD card, plug an ethernet cable to the A20 Micro, and proceed to the installation. You will not be lost: it's a standard netinstall image.

## Enable I2C

The module i2c-dev must be loaded:

    modprobe i2c-dev
		echo 'i2c-dev' >> /etc/modules
