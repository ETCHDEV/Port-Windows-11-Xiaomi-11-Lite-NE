<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Installing Windows

### Prerequisites

- [Windows on ARM image (Windows 11 is recommended)](https://uupdump.net/)
- [Latest UEFI image from release (or building one is recommended)](https://github.com/Project-Silicium/Mu-Silicium/releases/)
- [Instructions for building the UEFI image yourself](https://github.com/Project-Silicium/Mu-Silicium/blob/main/Building.md)
- [UEFI image for ONLY installing Windows!!](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/v0.0.1/boot-lisa-install.img)
- [Drivers](https://github.com/AistopGit/windows_oem_xiaomi_lisa) (Click on the Get Code arrow and Download as zip)
<!--- [DriverUpdater](https://github.com/WOA-Project/DriverUpdater/releases/latest) -->

#### Boot into modified UEFI img using the following command:
```cmd
fastboot boot boot-lisa-install.img
```
#### After booting into UEFI, select the UEFI boot menu and then in it select the last option (SCSI Disk option)

### Assigning letters to disks
#### Start the Windows disk manager from CMD as admin
> Once the Mi 11 Lite NE is detected as a SCSI Disk

```cmd
diskpart
```

### Assigning Mount Letter to Windows and BOOT Volumes

#### Select the Windows volume of the phone
> Use `list volume` to find it, it's the one named "WINLISA". Note the vol no.
```diskpart
select volume <number>
assign letter=w
```
#### Now select the ESP volume of the phone
> Use `list volume` to find it, it's the one named "ESPLISA". Note the vol no.

```diskpart
select volume <number>
assign letter=s
```
#### Now exit diskpart
```diskpart
exit
```

### Install
> Replace `<path/to/install.wim>` with the actual path to install.wim, 
> `install.wim` is located in sources folder inside your ISO (it might also be named `install.esd`), 
> You can get it either by mounting or extracting the ISO.
> Open install.wim with 7-zip and look at the XML file inside to check which index corresponds to which version of Windows

```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:<yourindexnumber> /ApplyDir:W:\
```

### Create Windows bootloader files

```cmd
bcdboot W:\Windows /s S: /f UEFI
```

### Install Drivers

> Replace `<lisadriversfolder>` with the actual location of the drivers folder
<!-- > If you want RDP, Follow this after the first boot -->

>Extract the zip file downloaded 
```cmd
.\driverupdater.exe -d <lisadriversfolder>\definitions\Desktop\ARM64\Internal\lisa.xml -r <lisadriversfolder> -p W:
```

>Or if you are in the folder
```cmd
.\driverupdater.exe -d definitions\Desktop\ARM64\Internal\lisa.xml -r . -p W:
```
  
## Allow unsigned drivers

> If you don't do this you'll get a BSOD

>  S: is the ESP partition
```cmd
cd S:\EFI\Microsoft\Boot
bcdedit /store BCD /set "{default}" testsigning on
bcdedit /store BCD /set "{default}" nointegritychecks on
bcdedit /store BCD /set "{default}" recoveryenabled no
bcdedit /store BCD /set "{default}" bootstatuspolicy IgnoreAllFailures
```

### Using RDP to control

> Don't use this if you want USB Host

Use the following guide: [RDP Method](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/guide/english/rdp-en.md)

### Unassign disk letters
> So that they don't stay there after disconnecting the device

Open CMD as Administrator:
```cmd
diskpart
```

#### Unassigning Volumes of the phone:
> Use `list volume` to find it, it's the one named "WINLISA"

```diskpart
select volume <number>
remove letter w
```

#### Now select the ESP volume of the phone
> Use `list volume` to find it, it's the one named "ESPLISA"

```diskpart
select volume <number>
remove letter s
```

#### Exit diskpart
```diskpart
exit
```

## Boot into Windows
### Now boot into the lastest UEFI img:

> ~~Use [boot-lisa-device.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-device.img) for device mode (or you need RDP and WinDbg)~~

> ~~Use [boot-lisa-host.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-host.img) for Host Mode (If you want to connect USB devices like USB Mouse)~~

> No need, Just use Mu-lisa.img.

```cmd
fastboot boot path/to/Mu-lisa.img
```

### Select the first Windows Option to boot

### If Windows rebooted after the setup to android, just go into fastboot the boot the img.
(I wouldn't recommend to flash the uefi img to boot because currently there is no support for majority of the device hardware and their drivers)

### ~~Use [boot-lisa-device.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-device.img) for device mode (or you need RDP and WinDbg)~~
### ~~Use [boot-lisa-host.img](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/releases/download/0.1/boot-lisa-host.img) for Host Mode (If you want to connect USB devices)~~
### No need, Just use Mu-lisa.img.

### You can launch programs by placing a batch script in startup folder. (Not required if usb host enabled)

### OR use [RDP Method](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/guide/english/rdp-en.md)
(Don't use this method if usb host required)

## Finished!
