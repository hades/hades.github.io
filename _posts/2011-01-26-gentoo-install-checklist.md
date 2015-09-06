---
layout: post
---
If you want to install Gentoo you should read the [Gentoo
Handbook](http://www.gentoo.org/doc/en/handbook/index.xml). However, if you
(like myself) have already installed Gentoo for a couple (thousand) times, you
know more or less everything that has to be done, but just need a list of
things not to forget. So here it is:

* `fdisk` # create the partitions you want
* `mkfs` # create the root filesystem
* `mount *root* /mnt/gentoo`
* `cd /mnt/gentoo`
* `wget -O- ftp://mirror.switch.ch/mirror/gentoo/releases/x86/current-stage3/stage3-i686-*.tar.bz2 | tar xj`
* `cd usr`
* `wget -O- ftp://mirror.switch.ch/mirror/gentoo/snapshots/portage-latest.tar.bz2 | tar xj`
* `cd /mnt/gentoo`
* `mount -t proc proc proc`
* `mount --bind /dev dev`
* `cp -L /etc/resolv.conf etc/`
* `chroot . /bin/bash`
* `env-update; source /etc/profile`
* `emerge --sync`
* `eselect profile set default/linux/x86`
* `vim /etc/make.conf` # setup USE, CFLAGS, CXXFLAGS, MAKEOPTS
* `vim /etc/locale.gen` # setup locales
* `locale-gen`
* `emerge gentoo-sources` # configure, build and install the kernel
* `vim /etc/fstab` # setup the partitions, don't forget root, swap
* `vim /etc/conf.d/hostname` # setup hostname
* `vim /etc/hosts` # add the hostname to localhost line
* `passwd`
* `vim /etc/conf.d/clock; emerge --config timezone-data` # setup timezone
* `emerge syslog-ng vixie-cron mlocate`
* `rc-update add syslog-ng default`
* `rc-update add vixie-cron default`
* `emerge reiserfsprogs xfsprogs jfsutils e2fsprogs` # or whatever set of FS you have
* `emerge dhcpcd`
* `emerge grub`
* `vim /boot/grub/grub.conf` # configure Grub
* `grep -v rootfs /proc/mounts > /etc/mtab`
* `grub`

Now reboot and smell the ashes!
