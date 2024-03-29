<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Troubleshooting Issues


## Device can boot into android but not bootloader

### Prerequisites:

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [parted for patitioning the storage](https://www.mediafire.com/file/s9bjano4pezphou/parted/file)

This is caused by partitions with volume names the bootloader cannot handle, to fix this:

- Boot to TWRP/OF or any custom recovery that supports device encryption/can access data partition and has adb support

- Copy the parted to the phone and enter adb shell using following commands:
```cmd
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```

- Start parted
```sh
cd /cache
./parted /dev/block/sda
```

- Run ```print``` to list all partitions

- Look for partitions that have spaces in the names e.g "Basic Data Partition" and note their volume number

- Now run ```rm <vol number>``` e.g ```rm 35``` (Make sure you deleted both the windows partitons created)

- If you deleted the partiton 34 and 35, follow the [Installation Guide](./partition-en.md) from Create the ESP partition step (Depending weather your device is 128GB or 256GB)

### If you dont have TWRP/OF Recovery only stock miui recovery then follow this method
- [Prerequisites](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/guide/english/troubleshooting-en.md#prerequisites)
- Boot to Miui Recovery
- Select "Connect with Mi Assistant"
- Open cmd and type following commands:
(```adb reboot fastboot``` redirects to fastbootd)
```cmd
adb reboot fastboot
fastboot flash boot <recovery.img>
fastboot reboot
```

- Once phone boots to recovery run following commands:
```cmd
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```

- Start parted
```sh
cd /cache
./parted /dev/block/sda
```

![image](https://github.com/TrustedFloppa/Port-Windows-11-Xiaomi-11-Lite-NE/assets/85476585/cb1849df-e222-462b-ae1c-74bf4f75fa02)

~~In this case i created too many Basic data partitions~~

Remove all of Basic Partition Data with ```rm <vol number>``` e.g ```rm 35```
If you removed Partition 33, 34, 35 then use following command or [Restore GPT](https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/guide/english/uninstall-en.md#restore-gpt)
```cmd
mkpart userdata ext4 12.1GB 125GB
```
