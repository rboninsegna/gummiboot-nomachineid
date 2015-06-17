# gummiboot-nomachineid
Gummiboot 48, without systemd

# Instructions

1. Download this repo with `git clone` or the compressed archives.
2. `sudo apt-get build-dep gummiboot` (should equal to: `aptitude install build-essential debhelper dh-autoreconf docbook-xsl gnu-efi libblkid-dev pkg-config xsltproc`)
3. Enter the `gummiboot-nomachineid-48` folder.
4. `dpkg-buildpackage -b -us -uc`
5. Install `gummiboot-nomachineid_48-100_amd64.deb` found in the parent folder.

Or cheat and try skipping to step 5 by downloading that file from Github's "releases" feature...
I'm not a Github master, sorry!

# Introduction

*Preface, which is a part of understanding this package: in Debian-like OSes, the kernel -- installed in /boot -- must be on a partition supporting Unix-like features. So you can't mount your EFI partition directly on /boot.*

*On the other hand, if you choose to encrypt your root partition but use GRUB2, you must split out your /boot partition anyway. On an EFI system, this means you need at least 3 partitions, one of which will be mostly empty even though it ought to be 200 MB according to EFI standards!*

This is the source code (in dpkg-buildpackage format) for gummiboot v48 (the current version in Debian stable Jessie), with one caveat: **all *machine-id* related code is stripped out** (except where that might make older files incompatible).

*machine-id is a 12-character hex string that for all intents and purposes is a form of UUID. Why I'm set against it will become clear later in this file.*

This key difference obviously has advantages and disadvantages, compared to upstream gummiboot:

* It does not act on *machine-id* rows in entry files :) -- this allegedly means you can't use it as a feature to selectively add some kernel arguments.

... and that would be all, if not for some antifeatures in Debian's adaptation where this version is arguably better:

* Kernels are now copied to {EFI partition}/linux-{version string}/{linux|initrd} instead of {EFI partition}/{machine-id}/{version string}/{linux|initrd}. Manual management becomes arguably easier.
* Respects the so-called "Unix philosophy" of well-focused software.
* Most notably, the adaptation has been [crippled by laziness of the Debian packagers](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=749706#15). While I wouldn't normally be pissed off by that, this resulted in an extremely strong dependency on systemd. Ironically, despite gummiboot and systemd having recently joined ways, this problem is not caused by shared code, but because systemd or dbus are the easiest way to generate a machine-id, which gummiboot requires.

Ironically again, this is a non-issue in Arch Linux, well known for being among the most progressist distros.

# Dedication

This package was unofficially developed on and for the [Devuan project](http://devuan.org/).



