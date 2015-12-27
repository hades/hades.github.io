---
layout: post
---
If you want to install Gentoo you should read the [Gentoo Handbook](http://www.gentoo.org/doc/en/handbook/index.xml). However, if you (like myself) have already installed Gentoo for a couple (thousand) times, you know more or less everything that has to be done, but just need a list of things not to forget. So here it is:

* <code>fdisk</code> # create the partitions you want
* <code>mkfs</code> # create the root filesystem
* <code>mount $ROOT /mnt/gentoo</code>
* <code>cd /mnt/gentoo</code>
* <code>wget -O- ftp://mirror.switch.ch/mirror/gentoo/releases/x86/autobuilds/current-stage3-i686/stage3-i686-*.tar.bz2 | tar xj</code>
* <code>cd usr</code>
* <code>wget -O- ftp://mirror.switch.ch/mirror/gentoo/snapshots/portage-latest.tar.bz2 | tar xj</code>
* <code>cd /mnt/gentoo</code>
* <code>mount -t proc proc proc</code>
* <code>mount --bind /dev dev</code>
* <code>cp -L /etc/resolv.conf etc/</code>
* <code>chroot . /bin/bash</code>
* <code>env-update; source /etc/profile</code>
* <code>emerge --sync</code>
* <code>eselect profile set default/linux/x86</code>
* <code>vim /etc/portage/make.conf</code> # setup USE, CFLAGS, CXXFLAGS, MAKEOPTS
* <code>vim /etc/locale.gen</code> # setup locales
* <code>locale-gen</code>
* <code>emerge gentoo-sources</code> # configure, build and install the kernel
* <code>vim /etc/fstab</code> # setup the partitions, don't forget root, swap
* <code>vim /etc/conf.d/hostname</code> # setup hostname
* <code>vim /etc/hosts</code> # add the hostname to localhost line
* <code>passwd</code>
* <code>vim /etc/timezone; emerge --config timezone-data</code> # setup timezone
* <code>emerge syslog-ng vixie-cron mlocate</code>
* <code>rc-update add syslog-ng default</code>
* <code>rc-update add vixie-cron default</code>
* <code>emerge reiserfsprogs xfsprogs jfsutils e2fsprogs</code> # or whatever set of FS you have
* <code>emerge dhcpcd</code>
* <code>emerge grub</code>
* <code>grub2-mkconfig > /boot/grub/grub.conf</code> # configure Grub
* <code>grep -v rootfs /proc/mounts > /etc/mtab</code>
* <code>grub2-install</code>

Now reboot and smell the ashes!
