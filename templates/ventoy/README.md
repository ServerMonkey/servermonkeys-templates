# servermonkeys Ventoy templates

## DESCRIPTION

A collection of my personal Ventoy templates for fully automated installation
of Debian and Windows. Includes Debian Preseed files and Windows Answer Files.

This guide will also show you how to create a USB-installation stick, for
fully automated/unattended installation. It will also show you how to use
fastpkg to download the necessary files.

## TEMPLATES

### Debain

* `generic.cfg` : Default Debian installation with SSHD enabled USER/PASS:
  ansible . Automatic DHCP discovery on the first Ethernet port. Swedish
  timezone/keyboard/locales. Includes proprietary firmware. Lets the user
  choose disk layout (semi automatic).
* `generic_laptop.cfg` : Same as generic, also installs 'laptop' package.
* `generic_nvme.cfg` : Same as generic, but automatically installs
  to `/dev/nvme*`
* `generic_nvme_laptop.cfg` : Same as `generic_nvme.cfg`
  plus `generic_laptop.cfg`
* `generic_sata.cfg` : Same as generic, but automatically installs
  to `/dev/sd*`
* `generic_sata_laptop.cfg` : Same as `generic_sata.cfg`
  plus `generic_laptop.cfg`

### Windows

Incomplete/untested

## INSTALLATION

Make sure you have installed the 'fastpkg' tool and added my public repo.
See: [github.com/ServerMonkey/fastpkg](https://github.com/ServerMonkey/fastpkg)

### Download requirements

Open a terminal and install Ventoy:

	sudo fastpkg -p ventoy install

To download all of the ISO files required by the ventoy.json file, run:

	sudo fastpkg -p "\
		debian-11-firmware-dvd1 \
		Windows-7-Pro \
		Windows-10-32bit \
		" download

This will automatically download the intended versions.

If you have problems downloading Windows ISO files read
here: [github.com/ServerMonkey/servermonkeys-devtools/windows-isos.md](https://github.com/ServerMonkey/servermonkeys-devtools/windows-isos.md)

You can also manually download all required ISO files and just rename each file
to the correct name provided in ventoy.json .

### Prepare USB stick

Get a USB-flash-drive with at least 8 GB of disk space.

Format the USB-stick with Ventoy:

	bash /opt/Ventoy_<VERSION>/Ventoy2Disk.sh /dev/<DISK>

To understand how to include the Pressed file in Ventoy, read this manual:  
[ventoy.net/en/plugin_autoinstall.html](https://www.ventoy.net/en/plugin_autoinstall.html)

Copy this entire folder to your USB sticks root partition and give it the
name 'ventoy'.

Copy all ISOs from the fastpkg downloads folder /var/lib/fastpkg/downloads to
the flash drives root partition.

You should have a disk layout similar to this:

```
/
- ventoy
   - debian
      - windows
      - README.md
      - ventoy.json
- debian-11-firmware-dvd1_11.1.0.x64.iso
- ...
```

Unmount your flash-drive. Its a good idea to make sure all ISO files are
copied, You can run the 'sync' command once before unmounting.

### BIOS/UEFI settings

The templates are configured to automatically select UEFI over BIOS but should
work with both.

## USAGE

Just put the flash-drive into you PC or server and select the menu option you
want.

## YOUR OWN DEBIAN PRESEEDS

To create your own Debain 11 Pressed file you can download a template with

	sudo fastpkg -p debian-11-preseed-example install

Or get the latest version here:  
[www.debian.org/releases/stable/example-preseed.txt](https://www.debian.org/releases/stable/example-preseed.txt)

For older Debian releases use Archive.org (check your Debian release date):  
[web.archive.org/web/*/https://www.debian.org/releases/stable/example-preseed.txt](https://web.archive.org/web/*/https://www.debian.org/releases/stable/example-preseed.txt)

Modify the Pressed file to your liking and include it in Ventoy.  
To understand how to modify the Pressed file, read this manual:  
[wiki.debian.org/DebianInstaller/Preseed](https://wiki.debian.org/DebianInstaller/Preseed)
