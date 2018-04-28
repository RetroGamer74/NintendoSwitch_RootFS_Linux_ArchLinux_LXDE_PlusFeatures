# Nintendo Switch RootFS Linux Arch Linux +LXDE + Features

This GitHub provides a RootFS https://drive.google.com/uc?export=download&confirm=no_antivirus&id=141TYSOAM9ZXrAnHTXaj47PjbiNP3zsgu which includes Arch Linux + LXDE, with Bluetooth support, so you can connect a keyboard and mouse. You can also connect your mobile phone or whatever. It supports WiFi, but this could be tricky. If you don't see wifi icon on LXDE after boot, try to setup your WiFi network and reboot again. Once rebooted you will see a black screen. It means you're in TegraRCM mode again. So inject again the exploit and payload to star linux again.

It includes also Chromium, Firefox, NFS support, SMB support, KODI but unfortunately still not working because of a crash I have to debug. Filezilla, and many others installed and compiled by myself. It also includes OnBoard keyboard, with touch screen capabilities already set it up. Start OnBoard when you want by entering in the Accesibility menu. Once launched, a new icon will appear in the desktop.

Probably first boot WiFi network doesn't work. That's completely normal according to the current status of this development. For the firs time, setup your network, and then reboot again. Usually first boot doesn't startup WiFi service because of a crash. I use bluetooth with iMac keyboard and Apple IntelliMouse with Bluetooth support. I use everything at the same time even with WiFi. And everything works well.

In order to do a complete installation in the easiest way I recommend follow next steps:

Following the installation instructions from the docker created by Nold360, which is the one who has created this automated procedure https://github.com/Nold360/switch_linux_kit do the next:

Cloning
=======

```
git clone https://github.com/Nold360/switch_linux_kit
cd switch_linux_kit
git submodule update --init
```

Compiling
=========

Note: You'll still need to get coreboot/tegra_mtc.bin on your own.

I don't support that file. It's not uploaded by myself.

https://www108.zippyshare.com/v/ZjHETezE/file.html

Copy the file in the coreboot folder.

Finally run docker command to start the procedure. If docker command is not found, you have to install it.

For example: sudo apt install docker
```
docker run -ti --rm -v$(pwd):/source nold360/switch_linux_toolchain bash 00_build.sh
```
microSD RootFS
==============

You have to prepare now a microSD card with Root FileSystem to start a complete linux system. ( minimum 8GB Size )

Put your microSD card in any adapter. SD adapter or USB adapter, and plug it into a computer with Linux OS.

Identify the new device using dmesg command for example, and taking a look to the last lines added.

Example:

```
[39400.150727] usb-storage 2-3:1.0: USB Mass Storage device detected
[39400.150776] scsi host6: usb-storage 2-3:1.0
[39400.150810] usbcore: registered new interface driver usb-storage
[39400.151700] usbcore: registered new interface driver uas
[39401.170628] scsi 6:0:0:0: Direct-Access     Generic  STORAGE DEVICE   0815 PQ: 0 ANSI: 6
[39401.171306] sd 6:0:0:0: Attached scsi generic sg3 type 0
[39401.484437] sd 6:0:0:0: [sdd] 124735488 512-byte logical blocks: (63.9 GB/59.5 GiB)
[39401.486509] sd 6:0:0:0: [sdd] Write Protect is off
[39401.486514] sd 6:0:0:0: [sdd] Mode Sense: 23 00 00 00
[39401.488487] sd 6:0:0:0: [sdd] Write cache: disabled, read cache: enabled, doesn't support DPO or FUA
```

Because I used an USB adapter my device was identified by sdd. You have to identify yours.

Run next commands as root to prepare microSD:
```
sudo fdisk /dev/sdd
```
Once in the fdisk command line you have to do the following:
```
Press 'p'. That shows the partition list.
```
If you haven't any partition that's perfect to follow next step.

If there are some partitions on list:
```
Press 'd' to delete partition. And after that select the number of partition to delete.
```
Repeat the procedure for removing a partition until no partitions left.
```
Now press 'n' to create a new partition.
Select 'p' for Primary partition.
Select a partition number, select 1.
Now select the size of the partition typing: +100M
```

This first partition will be dummy and won't be used.

Once this partition added, we have to create a new partition.

```
Press 'n' to create a new partition.
Select 'p' for primary.
Select partition number 2.
Press enter for partition size to use maximum size available in your microSD card.
```

Once finished, you can use command 'p' again to show the partition list.

You should see two partitions. First one about 100 megabytes, second one the rest of gigabytes available in your microSD card.

```
Then, finally, press 'w' to sync changes and exit.
```

Now let's go to format the new partition for linux use. Because I want to format the partition number 2, usually the identifier is the device identifier which was 'sdd' and adding the number of the partition which is 'sdd2'. So, type next command:

```
sudo mkfs.ext4 /dev/sdd2
```

If everything goes well you will see several messages about the construction of SUPERBLOCK and other things.

Now let's create a mount folder to get access to this new partition to write on it.

```
sudo mkdir /mnt/sdcard
```

Now mount the new partition.

```
sudo mount /dev/sdd2 /mnt/sdcard
```

We're ready to write in the folder /mnt/sdcard which is linked to our second partition of the microSD card.

Download now from this link https://drive.google.com/uc?export=download&confirm=no_antivirus&id=141TYSOAM9ZXrAnHTXaj47PjbiNP3zsgu the file rootfs_package.tar.bz2

And extract it into the sdcard. You can do this by downloading this file in to your home folder.

Then enter into the /mnt/sdcard:

```
cd /mnt/sdcard
```

Now extract the file guessing it was downloaded in your home folder:

```
sudo tar -xvf /home/retrogamer/rootfs_package.tar.bz2
```

This will take sometime depending on the speed of the microSD

Once finished run next commands:

```
cd
sudo umount /dev/sdd2
```

This umount command can take sometime to finish because it has to flush to the SDCard any pending data still not written to the microSD.

Once finished, you can extract the microSD card from your computer and adapter, and insert it into your Nintendo Switch socket.

Then using the Fusee Gelee method to shortcut pin 10-9 in the right joycon, and entering into TegraRCM, run the next commands:

```
sudo bash -x 02_exploit.sh
sudo bash -x 03_uboot.sh
```

The last command makes to appear the penguins on N.Switch screen after 5 or 6 seconds.















