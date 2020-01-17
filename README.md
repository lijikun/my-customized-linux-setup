# Preamble

Some notes and config files for setting up a linux desktop environment in my preferred way. Eventually it might be better to use  configuration management software such as Ansible instead.

Installing into a chroot using tools like of `debootstrap`, `pacstrap` or `rpm`/`yum`/`dnf`, rather than a installer, gives the user more control and minimizes the installation. 

General references: 
* https://wiki.archlinux.org/index.php/Installation_guide
* https://www.debian.org/releases/stable/amd64/apds03.en.html
* https://github.com/glacion/fedora-chroot-installation/blob/master/docs/VM-Install.md

# Before the First Reboot

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

# After the First Boot

* Put [customized config files](https://github.com/lijikun/my-linux-setup/tree/master/configfiles) like `.vimrc` or `.bashrc` into their respective places.

* Install other packages as needed.

* Customize monitors, desktop wallpapers, fonts, etc. 

* Apply workarounds and bugfixes like those mentioned in [notes](https://github.com/lijikun/my-linux-setup/tree/master/notes).
