If you wish to be able to use a bluetooth device, e.g. keyboard & mouse, in both OSes of your Windows/Linux dual boot system, you should first pair it in Linux, and then pair it in Windows. 

After that, extract the Bluetooth key from the Windows registry (e.g. with the `chntpw` tool under Linux), and edit the key into the bluetooth config files for this device under Linux, located in `/var/lib/bluetooth/[DEVICE MAC ADDRESS]/`.

For details see: https://unix.stackexchange.com/questions/255509/bluetooth-pairing-on-dual-boot-of-windows-linux-mint-ubuntu-stop-having-to-p
