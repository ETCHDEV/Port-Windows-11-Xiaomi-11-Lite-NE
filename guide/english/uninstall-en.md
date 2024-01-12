<img align="right" src="https://github.com/ETCHDEV/Port-Windows-11-Xiaomi-11-Lite-NE/blob/main/lisa.png" width="250" alt="Windows 11 Running On a Mi 11 Lite NE">


# Running Windows on the Mi 11 Lite NE/Mi 11 LE

## Uninstallation

### Why is this needed?

- If you have followed the old guide your partition order will be too different and may have some consequences if you dont restore your stock partition table.
- If you want to uninstall windows this is used instead of deleting partitions manually to avoid human error + writing a whole dedicated guide to just uninstalling.
- If you want to relock your bootloader you'll need your partition table to be stock.

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- gpt_both0.bin (Get it from the device's stock rom, located in images folder)

#### Restore GPT
> Replace ```<gpt_both0.bin>``` with the path to the gpt_both0.bin file.
```cmd
fastboot flash partition:0 <gpt_both0.bin>
```

#### Erase userdata to avoid bootloop and restore FS size
```cmd
fastboot -w
```

