# Notes on Installing a Highly Customized Linux Setup

## Preamble

This repo is just a collection of notes, links and config files for setting up a highly customized linux desktop environment in my preferred way. 

The general idea is to **forgo the official installers** of most Linux distros because they: (i) often have graphical interfaces that run into driver issues on newer hardware, and (ii) limit user options, especially on storage and package choice. Instead, I prefer installing into a **chroot** using tools like of `debootstrap`, `pacstrap` or `rpm`/`dnf`, i.e. installing every Linux distro the Arch/Gentoo way, while not really going into the trouble of building a LFS.

General insprations: 
* Arch: https://wiki.archlinux.org/index.php/Installation_guide
* Debian/Ubuntu: https://www.debian.org/releases/stable/amd64/apds03.en.html
* Fedora: https://github.com/glacion/fedora-chroot-installation/blob/master/docs/VM-Install.md

## Not-So-Easy Tips and Tricks

[Setting Up LUKS Encryption Including `/boot` on Debian/Ubuntu/Mint](https://cryptsetup-team.pages.debian.net/cryptsetup/encrypted-boot.html)

[Automatic Signing of DKMS-Generated Kernel Modules for Secure Boot (Nvidia Driver in CentOS 8 as Example)](https://gist.github.com/lijikun/22be09ec9b178e745758a29c7a147cc9)

## Creating the Base Chroot

* Boot with a live CD/USB. All Debian-based distros can be installed from any Debian-based live environments. For Fedora/Centos, choose "Troubleshooting" and "Rescue" options at boot to get into a "live" command-line interface.

* Connect to the Internet.

* Partition the storage devices on which the OS is to be installed with e.g. `parted` or `cgdisk`. Set up LVM, LUKS, RAID, etc now. Then format the partitions or logical volumes with `mkfs`.

* Mount the root partition. Create mountpoints for other partitions (e.g. `/boot`, `/boot/efi`, `/home`) and mount them if needed.

* Use `pacstrap`, `debootstrap`, `rpm`/`dnf`, etc to install the base system onto the mountpoint of the new root.

## Things That Should Be Done Before the First Boot

The basic chroot environment isn't bootable. The following must be done done after the minimal install. 

Properly mount/create the `dev`, `proc` and `sys` folders and chroot into the new installation, e.g. `LANG=en_US.UTF-8 chroot /mnt/target /bin/bash`:

* Set up the file systems, mainly by editing `/etf/fstab`.
    - If using swap file, create the swap file with `fallocate`, `mkswap` and `chmod`. Add the proper fstab line (`/path/to/swapfile none swap sw 0 0`).
    - Mount `/tmp` in the memory (fstab line: `tmpfs /tmp tmpfs rw,nosuid,nodev 0 0`) if necessary.
    - Bind-mount `/home`, `/var`, etc. if they are placed on separate partitions, disks or network locations. 

* Install a kernel.

* Install and configure the boot loader. If using `grub-pc`, edit `/etc/default/grub` for a sane grub config. If using `grub-efi`, `refind`, `systemd-boot`, etc., install it to EFI system partition.

* Set up locales with `locale-gen`, `localectl`, or `dpkg-reconfigure locales`. 

* Set the hardware clock to UTC with `hwclock`. There's a trick to [make Windows use UTC hardware clock](https://wiki.archlinux.org/index.php/System_time#UTC_in_Windows) if you dual boot.

    Configure the time zone by linking `/etc/localtime`, `timedatectl` or `dpkg-reconfigure tzdata`.

* Set the hostname.

* Change the root user password (or leave it empty to disable direct root login). Install `sudo` and create a normal user with sudo privilege. Also set default options for users. Put any necessary config files into `/etc/skel`, and edit `/etc/default/useradd`.

* Install necessary proprietary firmwares and drivers (especially for WiFi and GPU) so that one has a usable system on first boot. 

* Set up network connection(s), e.g. by copying the contents of `/etc/NetworkManager/system-connections` to the new system.

* Install essential tools such as `ssh`, `vim` and `tmux` in case you need to troubleshoot, as well as Windows compability utilities such as `ntfs-3g` if you dual boot.

* Install a desktop environment if necessary.

## Stuff that Can Be Done After the First Boot

* Disable onboard audio (`echo 'blacklist snd_hda_intel' | sudo tee /etc/modprobe.d/disable-onboard-audio.conf`) if necessary. Also set up correct sampling rate for sound devices such as USB sound cards.

* Put [customized config files](https://github.com/lijikun/my-linux-setup/tree/master/configfiles) like `.vimrc` or `.bashrc` into their respective places.

* Install other packages as needed.

* Customize monitors, desktop wallpapers, fonts, etc. 

* Apply workarounds and bugfixes like those mentioned in [notes](https://github.com/lijikun/my-linux-setup/tree/master/notes).
