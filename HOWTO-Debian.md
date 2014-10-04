Debian Setup Instructions
=========================

This document describes how to get Debian and a simple C-based PubNub
demo that flashes an LED running on your SAMA5D3 Xplained development
board.

Contrary to the Poky system, you can develop, prototype and build your
PubNub-based software directly on the ARM board; it has plenty enough
RAM and CPU power to compile smaller software packages.


SD Card Setup
-------------

The Debian system is not installed to the internal NAND flash but to an
SD card; any normal SD card should do, use at least 1GB sized card.

You will need a UNIXy system on another computer with SD card drive
to set up the card; in an extreme case, even the Poky build flashed
on your Xplained board in the factory could suffice, but we will not
describe the process in detail.

In the following, we assume a Linux PC system.  If you don't have
a dedicated SD drive, some microSD cards come with an SD adapter
*and* USB adapter.

Note that these instructions are heavily based on a more technical howto

	https://eewiki.net/display/linuxonarm/ATSAMA5D3+Xplained

that you can follow to also build your own bootloader and kernel.

## Partitioning SD Card

Typical SD cards have just a single partition that holds all the data.
In order to create ARM-bootable SD card, we will need two partitions;
a 48MB boot FAT partition (type 0xE) that holds the bootloader and Linux
kernel, and an ext4 partition (type 0x83) covering the rest of the
device that holds the Debian root filesystem.

We will assume that the SD card device name is /dev/mmcblk0 - it can
also be something like /dev/sdx or such.  The `lsblk` tool can help
you identify the device name.  Be careful, using the wrong device name
can cause overwriting your other disk drives!

Cut'n'paste the following commands turn by turn to set up the SD card
partitions:

	export DISK=/dev/mmcblk0
	sudo dd if=/dev/zero of=${DISK} bs=1M count=50
	sudo sfdisk --in-order --Linux --unit M ${DISK} <<-__EOF__
	1,48,0xE,*
	,,,-
	__EOF__
	DISKP=$DISK
	case ${DISKP} in *[0-9]) DISKP=${DISKP}p;; esac

## Setup Linux Boot Partition

We have already prepared a boot partition that you can simply flash
on your SD card.  Follow the eewiki.net howto to build it from scratch.

	wget -c http://pasky.or.cz/dev/pubnub/SAMA5D3X-boot.img
	sudo dd if=SAMA5D3X-boot.img of=${DISKP}1 bs=1M

## Setup Debian Root Partition

We will use the eewiki-provided Debian root filesystem:

	wget -c https://rcn-ee.net/deb/minfs/wheezy/debian-7.5-minimal-armhf-2014-07-07.tar.xz
	tar xf debian-7.5-minimal-armhf-2014-07-07.tar.xz

We provide an additional set of files that covers kernel-specific
data and some config file updates:

	wget -c http://pasky.or.cz/dev/pubnub/SAMA5D3X-files.tar.xz

Now, we will store the data on the SD card:

	sudo mkfs.ext4 ${DISKP}2 -L rootfs
	mount ${DISKP}2 /mnt
	sudo tar xfvp ./*-*-*-armhf-*/armhf-rootfs-*.tar -C /mnt
	sudo tar xfvp SAMA5D3X-files.tar.xz -C /mnt
	umount /mnt
	sync

## Booting Up

At this point, we can insert the SD card in the development board
and power up.  With SD card present, the board will automatically
select it for boot instead of the internal flash.

Unfortunately, the serial console doesn't seem to work during boot;
however, after boot is finished, a login prompt will appear on the
serial console (should appear as /dev/ttyACM0 if you are powering
the board over USB from a Linux PC).

To login over a network, after about 30 seconds after power-on,
the system will request an IP address over DHCP and after about
further 90 seconds, ssh login will become available.  If DHCP is
inconvenient in your setting, you can configure a static IP address
in etc/network/interfaces on the Debian root filesystem.

Use root:root or debian:temppwd to login.


PubNub Setup
------------

In all the following steps, we assume you are logged in on the board
(we recommend to login as the `debian` user).

First, let's install git and basic C build environment:

	sudo apt-get install git build-essential

Now, let's clone the PubNub C SDK:

	git clone https://github.com/pubnub/c
	cd c

As described in its README, we will install its dependencies:

	sudo apt-get install libevent-dev libjson0-dev libcurl4-openssl-dev libssl-dev

Now, we can build and install the SDK:

	make
	sudo make install

And let's try some PubNub client example:

	cd examples/libevent-sub
	make
	./example-libevent-sub

(and hit the 'a' key now)


TODO: Make archives available.
TODO: A board-specific LED-flashing demo.
