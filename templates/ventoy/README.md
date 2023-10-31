# servermonkeys Ventoy templates

## DESCRIPTION

A collection of my personal Ventoy templates for fully automated installation
of Debian and Windows. Includes Debian Preseed files and Windows Answer Files.

This guide will also show you how to create a USB-installation stick, for
fully automated/unattended installation. It will also show you how to use
fastpkg to download the necessary files.

## TEMPLATES

### Debian 11/12

* `generic.cfg` : Default Debian installation with SSHD enabled USER/PASS:
  _ansible_ . Automatic DHCP discovery on the first Ethernet port. Swedish
  timezone/keyboard/locales. Includes proprietary firmware. Lets the user
  choose disk layout (semi automatic).
* `generic_laptop.cfg` : Same as generic, also installs 'laptop' package.
* `generic_nvme.cfg` : Same as generic, but automatically installs
  to `/dev/nvme*`
* `generic_nvme_laptop.cfg` : Same as `generic_nvme.cfg`
  plus `generic_laptop.cfg`
* `generic_sda.cfg` : Same as generic, but automatically installs
  to `/dev/sda`. Be careful this can target and erase the Ventoy USB-stick
  itself. If you are unsure choose `generic.cfg` instead.
* `generic_sda_laptop.cfg` : Same as `generic_sda.cfg`
  plus `generic_laptop.cfg`

### Windows

Incomplete/untested

### Prepare USB stick

Get a USB-flash-drive with at least 8 GB of disk space.

Format the USB-stick with Ventoy:

	bash /opt/Ventoy_<VERSION>/Ventoy2Disk.sh /dev/<DISK>

To understand how to include the Pressed file in Ventoy, read this manual:  
[ventoy.net/en/plugin_autoinstall.html](https://www.ventoy.net/en/plugin_autoinstall.html)

Copy this entire folder to your USB sticks root partition and give it the
name 'ventoy'.

Copy all ISO files required by the ventoy.json file to the flash drives root
partition.

You can use the tool fastpkg to download the correct disk
iso's [github.com/ServerMonkey/fastpkg](https://github.com/ServerMonkey/fastpkg)

Open the file ventoy.json file and lok at the lines that contain the key '
image'. The run for example:

	sudo fastpkg -p debian-11-firmware-dvd1_11.1.0.x64 download

Then copy onto the mounted USB-stick.  
On some Linux distributions it is a good idea to run the 'sync' command to make
sure all files are fully written to the USB-stick.

    rsync -aP /var/lib/fastpkg/downloads/debian-11-firmware-dvd1_11.1.0.x64.iso /mnt/usbstick
    sync

If you have problems downloading Windows ISO's read
here: [github.com/ServerMonkey/servermonkeys-devtools/windows-isos.md](https://github.com/ServerMonkey/servermonkeys-devtools/windows-isos.md)

Now you should have a disk layout similar to this:

```
/
- ventoy
   - debian/
   - windows/
   - README.md
   - ventoy.json
- debian-11-firmware-dvd1_11.1.0.x64.iso
- ...
```

Unmount your flash-drive and you are done.

### BIOS/UEFI settings

The templates are configured to automatically select UEFI over BIOS but should
work with both depending on how you boot into the Ventoy USB-stick.

## USAGE

Just put the flash-drive into your PC or server and select the menu option you
want.

## YOUR OWN DEBIAN PRESEEDS

To create your own Debain Pressed file you can download a template with

	sudo fastpkg -p debian-11-preseed-example install

Or get the latest version here:  
[www.debian.org/releases/stable/example-preseed.txt](https://www.debian.org/releases/stable/example-preseed.txt)

For older Debian releases use Archive.org (check your Debian release date):  
[web.archive.org/web/*/https://www.debian.org/releases/stable/example-preseed.txt](https://web.archive.org/web/*/https://www.debian.org/releases/stable/example-preseed.txt)

Modify the Pressed file to your liking and include it in Ventoy.  
To understand how to modify the Pressed file, read this manual:  
[wiki.debian.org/DebianInstaller/Preseed](https://wiki.debian.org/DebianInstaller/Preseed)
