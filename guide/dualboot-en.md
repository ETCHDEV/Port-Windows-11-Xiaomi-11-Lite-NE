<img align="right" src="https://github.com/woa-vayu/src_vayu_windows/blob/main/2Poco X3 Pro Windows.png" width="350" alt="Windows 11 Running On A Poco X3 Pro">


# Running Windows on the POCO X3 Pro

## Dualbooting Android and Windows seamlessly (Option 1)

> **Note** This will work on any android version and kernel but it does take longer to boot into android from windows

### Prerequisites

- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [TWRP/OFOX](../../../releases/Recoveries)

- [NTFS Android Magisk Module](../../../releases/ntfsdroid)

- [UEFI](https://github.com/woa-vayu/edk2-msm/releases/latest)

- [UEFI & Android Boot Flashing App](../../../releases/dualboot)

### Phone Setup

#### Adding NTFS Support to android

- Install Magisk if you haven't already
- Install the NTFS Android magisk module through the Magisk manager

#### Application Setup

- Install the APK provided
- Create a folder named "UEFI" on your internal storage
- Copy all uefi images into that folder with the naming scheme ```vayu-<panel>-<version>.img```
- Open the app and allow any root access it wants

### PC part

#### Flashing the recovery and booting it

- Reboot to the bootloader

- Run ```fastboot flash recovery <recovery.img> reboot recovery```

#### Transfering files to the phone

- When the recovery has booted run ```adb shell mount.ntfs /dev/block/by-name/win /win```
- Run ```adb shell dd if=/dev/block/by-name/boot of=/win/boot.img```
- Run ```adb push switchtoandroid.exe /win/Users/<username>/Desktop/switchtoandroid.exe```
  
#### Booting to android
  
  - Run switchtoandroid.exe on the phone as **ADMINISTRATOR**

#### Booting to windows
  
  - Run the app
  - Press "Quickboot to windows"
  
## Finished!
  
  

  
## Dualbooting Android and Windows seamlessly (Option 2)
  
> **Note** This may not work on some android kernels or versions, you will have to redo this every update or rom install and sometimes it may randomly break meaning you have to reflash but it is easier and faster to boot into android
  
### Prerequisites

- [Latest installer ZIP](https://github.com/Woa-Vayu/edk2-msm/releases/latest)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [TWRP/OFOX](../../../releases/Recoveries)
  
  
#### Flashing the recovery and booting it

- Reboot to the bootloader

- Run ```fastboot flash recovery <recovery.img> reboot recovery```

  
#### Putting your phone into sideload mode
  
- Go to the advanced tab on your phone
- Press ADB Sideload
- Swipe to start sideload mode
  
#### Flashing UEFI With dualboot support
  
- On your PC open the command prompt
- In the command prompt do ```adb sideload <pathtouefizip>```
