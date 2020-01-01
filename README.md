# installation guides

https://www.youtube.com/watch?v=fA9AotXqkqA

https://techsprobe.com/dual-boot-macos-mojave-with-windows-10/

# SMBIOS

Serial number must be invalid
![Image](https://github.com/hiyongrui/hackintosh/blob/master/invalid_serialnumber.png?raw=true)

https://checkcoverage.apple.com

https://everymac.com/ultimate-mac-lookup/

Generate SmUUID in terminal using 
```
uuidgen command
```

# clover installer
https://mackie100projects.altervista.org/download-clover-configurator/
https://github.com/CloverHackyColor/CloverBootloader/releases

# updating from 10.15.1 > 10.15.2 worked, but the next day somehow my boot options were gone
ended up creating another volume and install macos from usb - was able to boot, hence not efi folder fault
after searching for quite some time (keyword clover filevault boot gone), realised I had somehow turn on FileVault during the update i think, checked status using
```
diskutil apfs list
```
to turn off FileVault refer to https://bigendians.com/2019/01/11/hackintosh-apfs-and-filevault-2-encryption-boot-problems/
