# [Asus K401UQ](https://www.asus.com/Laptops/K401UQ/) hackintosh laptop
#### Installation references for my vanilla hackintosh with Clover - dual boot macOS Catalina and Windows 10

![Image](https://github.com/hiyongrui/hackintosh/blob/master/Resources/about_this_mac.png)

## Modern updated vanilla guide for hackintosh laptop
- https://fewtarius.gitbook.io/laptopguide/

## Tools used: Clover Configurator, Clover EFI Bootloader, MaciASL, Hackintool, IORegistryExplorer
- https://mackie100projects.altervista.org/download-clover-configurator/
- https://github.com/CloverHackyColor/CloverBootloader/releases
- https://github.com/acidanthera/MaciASL
- https://github.com/headkaze/Hackintool

## Important kexts to update for every macOS version
- https://github.com/acidanthera/WhateverGreen
- https://github.com/acidanthera/Lilu
- https://github.com/acidanthera/AppleALC

## Installation process
- step 1 installing macOS on virtual machine [VirtualBox](https://www.youtube.com/watch?v=8C4GSIVWGck)
- step 2 installing macOS on USB inside VirtualBox (require > 16 GB due to catalina installation file size)
- step 3 installing [Windows 10](https://www.microsoft.com/en-gb/software-download/windows10ISO) on another USB from Windows laptop

- https://www.youtube.com/watch?v=fA9AotXqkqA
- https://techsprobe.com/dual-boot-macos-mojave-with-windows-10/

## SMBIOS
- Serial number must be invalid
![Image](https://github.com/hiyongrui/hackintosh/blob/master/Resources/invalid_serialnumber.png)

- https://checkcoverage.apple.com
- https://everymac.com/ultimate-mac-lookup/

- Generate SmUUID in terminal using 
```
uuidgen command
```

## updating from 10.15.1 > 10.15.2 worked, but the next day somehow my boot options were gone
- update have to select - data partition https://www.tonymacx86.com/threads/macos-catalina-10-15-0-supplemental-update.285293/

- ended up creating another volume and install macos from usb - was able to boot, hence not efi folder fault
after searching for quite some time (keyword clover filevault boot gone), realised I had somehow turn on FileVault during the update i think, checked status using
```
diskutil apfs list
```
- to turn off FileVault refer to https://bigendians.com/2019/01/11/hackintosh-apfs-and-filevault-2-encryption-boot-problems/

## Battery life optimization
- CPUFriend + CPUFriendFriend, check Intel Power Gadget min frequency before optimise was > 1
    - https://fewtarius.gitbook.io/laptopguide/battery-power-management/optimizing-battery-life

- more detailed way to understand how CPUFriend worked 
    - https://olarila.com/forum/viewtopic.php?t=8268

## Brightness consistency
- f1, f2 brightness increment/decrement key mapping using https://github.com/pqrs-org/Karabiner-Elements
- brightness consistency everytime when boot use [WhateverGreen](https://github.com/acidanthera/WhateverGreen) and compile its SSDT-PNLF

## Wifi
- https://khronokernel-7.gitbook.io/wireless-buyers-guide/types-of-wireless-card/usb
- for USB wifi, prevent [HalUsbInMpdu() log](https://github.com/chris1111/Wireless-USB-Adapter-Clover/issues/51) during booting and shutdown use v10 version instead of latest v11

## Dual boot Windows 10
- from BIOS, set clover UEFI to be default boot option priority 1, Catalina macOS to be default boot volume
    - https://hackintosh-multiboot.gitbook.io/hackintosh-multiboot/uefi/1-disk-+-no-os-macos
        - PS: bypass sign in microsoft option by disabling wifi

- rename custom boot entry option
    - https://www.reddit.com/r/hackintosh/comments/2k0wi0/clover_rename_os_x_volume/
    - https://www.reddit.com/r/hackintosh/comments/40afda/customization_how_to_get_rid_of_certain_drives_on/

- time synchronization of both OS
    - https://www.reddit.com/r/hackintosh/comments/6hx05v/is_there_a_fix_for_windows_changing_its_time_when/

- hide hidden volume at clover boot
    - open config.plist, go to Gui, at "Hide Volume" on the right enter "Recovery", "Preboot", at clover if wan to see all press F3

## Trackpad fix
- https://voodooi2c.github.io/#Installation/Installation
- kext = VoodooI2CFTE.kext, patched first 2 digit 6D = 55, method have FTE support (previously only ELAN) copied from page 14
    - https://www.tonymacx86.com/threads/voodooi2c-help-and-support.243378/page-14
- [example of process for Asus ROG GL552](https://github.com/fidele007/Asus-ROG-GL552VW-Hackintosh/wiki/TouchPad-with-VoodooI2C)

## Audio fix
- https://hackintosher.com/guides/get-hackintosh-audio-working/

## Battery fix
- https://olarila.com/forum/viewtopic.php?t=8208
    - kext = ACPIBatteryManager.kext
    - patched using DSDT with [MaciASL](https://github.com/acidanthera/MaciASL/releases)
        - steps in [dsdt_patchcodes.txt](https://github.com/hiyongrui/hackintosh-asus/blob/master/Resources/dsdt_patchcodes.txt), DSDT attached is the patched one.

## USB port mapping
- using either [Hackintool](https://github.com/headkaze/Hackintool) or [USBMap](https://github.com/corpnewt/USBMap)
- disabled VGA webcam (HS06) and internal bluetooth (HS08)

# Other resources used for reference to understand hackintosh as laptop [gitbook](https://fewtarius.gitbook.io/laptopguide/) was not released when i started #

### alternative brightness consistency fix
- method 1 brightness fix - https://github.com/PoomSmart/ASUS-FX504GE-Hackintosh
    - CLOVER/APCI/PATCHED = [SSDT-PNLF.aml](https://github.com/hiyongrui/hackintosh-asus/blob/master/Resources/SSDT-PNLF.aml) and [SSDT-PNLFCFL.aml](https://github.com/hiyongrui/hackintosh-asus/blob/master/Resources/SSDT-PNLFCFL.aml) attached
- method 2 https://www.elitemacx86.com/threads/guide-how-to-enable-backlight-control-on-laptop.182/
/other folder = AppleBacklightFixup.kext, CLOVER/ACPI/PATCHED = SSDT-PNLF.aml, rename GFX0 to IGPU in config.plist
remap keyboard so f1 f2 brightness increment/decrement work using Karabiner Elements

### alternative disable nvidia
- https://www.tonymacx86.com/threads/guide-asus-k501uq-high-sierra.243518/

### boot from black screen installation stuck
- https://www.tonymacx86.com/threads/solved-gioscreenlockstate-3-hs-0-bs-0-now-0-sm-0x0.264188/
- https://www.elitemacx86.com/threads/fix-intel-hd-graphics-520-and-620-on-macos-10-14-4.390/

### extras
- https://medium.com/@ayushere/common-problems-and-workarounds-in-hackintosh-85d76ad89d24
- https://wallpapershome.com/art/illustrations/abstract-nature-8k-21456.html
- https://github.com/agarrharr/awesome-macos-screensavers
- https://www.tonymacx86.com/threads/hp-spectre-do-i-need-usbinjectall-and-or-port-limit-patch-if-laptop-only-has-3-usb-ports-and-1-of-them-is-thunderbolt.285559/
- https://www.tonymacx86.com/threads/apple-chime-at-startup-added-to-clover.277362/page-4
