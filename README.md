# Notes on Installing a Highly Customized Linux Setup

## Preamble

This repo is just a collection of notes, links and config files for setting up a highly customized linux desktop environment in my preferred way. 

The general idea is to forgo the official installers of various Linux distros because they: (i) often have graphical interfaces that run into driver issues on newer hardware, and (ii) limit user options, especially by limiting storage options and package choices. Instead, I prefer installing into a chroot using tools like of `debootstrap`, `pacstrap` or `rpm`/`yum`/`dnf`, i.e. installing every Linux distro the Arch/Gentoo way, while not really going into the trouble of building a LFS.

General insprations: 
* Arch: https://wiki.archlinux.org/index.php/Installation_guide
* Debian/Ubuntu: https://www.debian.org/releases/stable/amd64/apds03.en.html
* Fedora: https://github.com/glacion/fedora-chroot-installation/blob/master/docs/VM-Install.md

## Not-So-Easy Tips and Tricks

[Setting up LUKS encryption including `/boot` on Debian/Ubuntu/Mint](https://cryptsetup-team.pages.debian.net/cryptsetup/encrypted-boot.html)

[Automatic Signing of DKMS-Generated Kernel Modules for Secure Boot (Nvidia Driver in CentOS 8 as Example)](https://gist.github.com/lijikun/22be09ec9b178e745758a29c7a147cc9)

## Before the First Reboot

The following are done after the minimal install. Properly mount/create the `dev`, `proc` and `sys` folders and chroot into the new installation, e.g. `LANG=en_US.UTF-8 chroot /mnt/target /bin/bash`:

* Set up the file systems, mainly by editing `/etf/fstab`.
    - If using swap file, create the swap file with `fallocate`, `mkswap` and `chmod`. Add the proper fstab line (`/path/to/swapfile none swap sw 0 0`).
    - Mount `/tmp` in the memory (fstab line: `tmpfs /tmp tmpfs rw,nosuid,nodev 0 0`) if necessary.
    - Bind-mount `/home`, `/var`, etc. if they are placed on separate partitions, disks or network locations. 

* Edit `/etc/locale.gen` as necessary. Set up locales with `locale-gen` command. 

* Set the hardware clock to UTC with `hwclock`. Configure the time zone.

* Set the hostname.

* Change the root user password, and/or create a normal user with sudo privilege. Also set default options for users. Put any necessary stuff into `/etc/skel`, and edit `/etc/default/useradd`.

* Install necessary proprietary firmwares and drivers (especially for WiFi and GPU) so that one has a usable system on first boot. 

* Set up network connection(s), e.g. by copying the contents of `/etc/NetworkManager/system-connections` to the new system.

* Install important packages such as `sudo`, `ssh` and `vim`, as well as Windows compability utilities such as `ntfs-3g`.

* Install and configure the boot loader. If using `grub-pc`, edit `/etc/default/grub` for a sane grub config. If using `refind` or `efibootmgr`, install it to EFI system partition.

* Install a desktop environment if necessary.

* Disable onboard audio (`echo 'blacklist snd_hda_intel' | sudo tee /etc/modprobe.d/disable-onboard-audio.conf`) if necessary. Also set up correct sampling rate for sound devices such as USB sound cards.

## After the First Boot

* Put [customized config files](https://github.com/lijikun/my-linux-setup/tree/master/configfiles) like `.vimrc` or `.bashrc` into their respective places.

* Install other packages as needed.

* Customize monitors, desktop wallpapers, fonts, etc. 

* Apply workarounds and bugfixes like those mentioned in [notes](https://github.com/lijikun/my-linux-setup/tree/master/notes).
