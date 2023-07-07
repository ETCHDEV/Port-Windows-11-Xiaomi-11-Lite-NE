<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="350" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Installation

## Partitioning your device

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [parted for patitioning the storage](example.com)

### Notes:
> **Warning** if you ever delete any partitions via diskpart, Windows will send a UFS command which would erase the entire UFS storage!
- All your data will be erased! Backup now if needed.
- These commands have been tested.
- Ignore `udevadm` warnings.
- Do not run the same command twice.
- DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the Renegade Telegram.

#### ⚠️ Do not run all commands at once, execute them in order!

#### ⚠️ DO NOT MAKE ANY MISTAKE!!! YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

##### Boot to TWRP/OF or any custom recovery that supports device encryption/can access data partition and has adb support
##### Copy the parted to the phone and enter adb shell using following commands:
```cmd
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```

WORK IN PROGRESS ⚠️

## [Next step: Installing Windows](/guide/install-en.md)
