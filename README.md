Some notes and config files for setting up a linux desktop environment in my preferred way. Eventually it might be better to use  configuration management software such as Ansible instead.

It gives more control if one uses the commands line with the likes of `debootstrap`, `pacstrap` or `rpm`/`yum`/`dnf`, rather than a installer. 

General references: 
* https://wiki.archlinux.org/index.php/Installation_guide
* https://www.debian.org/releases/stable/amd64/apds03.en.html
* https://github.com/glacion/fedora-chroot-installation/blob/master/docs/VM-Install.md

# Before the First Reboot

The following are done after basic manual installation or installer. Properly mount `dev`, `proc` and `sys` folders and chroot into the new installation:

* Set up the file systems, mainly by editing `/etf/fstab`.
    - Create the swap file and the proper fstab line (`/path/to/swapfile none swap sw 0 0`) if no swap partition is used.
    - Mount `/tmp` in the memory (fstab line: `tmpfs /tmp tmpfs rw,nosuid,nodev 0 0`) if necessary.
    - Bind-mount `/home`, `/var`, etc. if they are placed on separate partition(s). 

* Edit `/etc/locale.gen` as necessary. Set up locales with `locale-gen` command. 

* Set the hardware clock to UTC. Configure the time zone.

* Set the hostname.

* Install necessary proprietary firmwares and drivers (especially for WiFi and GPU) so that one has a usable system on first boot. 

* Set up network connection(s), e.g. by copying the contents of `/etc/NetworkManager/system-connections` to the new system.

* If using `grub-pc`, edit `/etc/default/grub` for a sane grub config. If using `refind`, install it to EFI system partition.

* Install key packages such as `sudo`, `ssh` and `vim`, as well as Windows compability utilities such as `ntfs-3g`.

* Change the root user password, and/or create a normal user with sudo privilege.

* Install a desktop environment if necessary.

* Disable onboard audio (`echo 'blacklist snd_hda_intel' | sudo tee /etc/modprobe.d/disable-onboard-audio.conf`) if necessary. Also set up correct sampling rate for sound devices such as USB sound cards.
