<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Installation

## Partitioning your device

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [parted for patitioning the storage](https://www.mediafire.com/file/s9bjano4pezphou/parted/file)

### Notes:
> **Warning** if you ever delete any partitions via diskpart, Windows will send a UFS command which would erase the entire UFS storage!
- All your data will be erased! Backup now if needed.
- These commands have been not been completely tested (Only for the 256GB varient device)
- Do not run the same command twice.
- DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help on my Telegram.

#### ⚠️ Do not run all commands at once, execute them in order!

#### ⚠️ DO NOT MAKE ANY MISTAKE!!! YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

##### Boot to TWRP/OF or any custom recovery that supports device encryption/can access data partition and has adb support
##### Copy the parted to the phone and enter adb shell using following commands:
```cmd
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```

##### Resize the partition table
> So that the Windows partitions can fit
```sh
sgdisk --resize-table 64 /dev/block/sda
```

##### Start parted
```sh
cd /cache
./parted /dev/block/sda
```

##### Delete the `userdata` partition
> You can make sure that 33 is the userdata partition number by running
>  `print all`
```sh
rm 33
```

##### Creating the partitions
> If you get any warning message telling you to ignore or cancel, just type i and press Enter

<details>
<summary><b><strong>For 128GB Models</strong></b></summary>



- Create Android's data partition
```sh
mkpart userdata ext4 12.2GB 70.0GB
```

- Create the ESP partition (stores Windows bootloader data and EFI files)
```sh
mkpart esp fat32 70.0GB 70.3GB 
```

- Create the main partition where Windows will be installed to
```sh
mkpart win ntfs 70.3GB 125GB
```

  </summary>
</details>

<details>
<summary><b><strong>For 256GB Models</strong></b></summary>


- Create Android's data partition(Find where the last part end using print and start the userdata part from there)
```sh
mkpart userdata ext4 12.2GB 140.2GB
```

- Create the ESP partition (stores Windows bootloader data and EFI files)
```sh
mkpart esp fat32 140.2GB 140.5GB
```

- Create the main partition where Windows will be installed to
```sh
mkpart win ntfs 140.5GB 254GB
```
  </summary>
</details>

##### Make ESP partiton bootable so the UEFI image can detect it and quit
```sh
set 34 esp on
quit
```
##### Reboot to the custom recovery
##### Start the shell again on your PC
```cmd
adb shell
```

##### Format partitions
-  Format the ESP partiton as FAT32
```sh
mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPLISA
```

-  Format the Windows partition as NTFS
```sh
mkfs.ntfs -f /dev/block/by-name/win -L WINLISA
```
- Format data
Go back and to "Wipe" menu and press "Format Data", 
then type `yes`.

##### Just restart the phone, and see if Android still works (Yes, the phone is formatted)

### The hard part is done!!, Now go to

## [Next step: Installing Windows](./install-en.md)
