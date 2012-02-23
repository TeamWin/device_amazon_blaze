# CM7 for Kindle Fire

## Info

|||
|-----------------------------------:|:--------------------------|
|**Discussion and build thread** | http://forum.xda-developers.com/showthread.php?t=1508802
|**Whistlestop's Discussion and build thread (deprecated)** | http://forum.xda-developers.com/showthread.php?t=1390773
|**Skimp Killah's binary build (possibly deprecated?)** | http://forum.xda-developers.com/showthread.php?t=1472609
|**Takenover83's 'Custom TWRP backup' (for TWRP only, this should be obvious** | http://forum.xda-developers.com/showthread.php?t=1475054

## Building 

### Initialize
[setup linux/OS X](http://source.android.com/source/initializing.html) please note: it must be sun-java-6, not openjdk

### Create a Working directory and download Google's repo command

```bash
mkdir cm7
cd cm7/
curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

### Sync with modified CyanogenMod tree.
```bash
repo init -u git://github.com/IngCr3at1on/platform_manifest.git -b gingerbread
repo sync -j16
```

### If you would like to build for TWRP (remove the reboot recovery option)
```bash
repo init -u git://github.com/IngCr3at1on/platform_manifest.git -b gingerbread-twrp
repo sync -j16
```

### Compile
```bash
source build/envsetup.sh
brunch blaze -j$(grep -c processor /proc/cpuinfo)
```

This should produce a flashable out/target/product/blaze/cm-7.2.0-RC0-blaze-KANG-signed.zip file. 
If the signing process fails try to run it again (the following command is extrapolated from the CM9 walkthrough and I have not tested it).

```bash
./device/amazon/blaze/releasetools/ota_from_target_files -v \
           -p out/host/linux-x86 \
           -k build/target/product/security/testkey \
           --backup=true \
           --override_device=auto \
           out/target/product/blaze/obj/PACKAGING/target_files_intermediates/cyanogen_blaze_target_files-eng.$USER.zip out/target/product/blaze/cm-7.2.0-RC0-blaze-KANG-signed.zip
```

###Notes for compiling on Mac OS X (by davidnintendo)
* Xcode 4 may not work. Use Xcode 3 instead. You may need to modify the installer to get it working on Lion, though. Use Google to get instructions. 
* MacPorts didn't work well for me either. Homebrew worked better. 
* If your drive is on a non case-sensitive format, you will need to use a disk image to build. Make sure to have enough space on it to compile because everything including intermediate files will be created in the disk image. 40 GB will be enough. Also, if you don't want those 40 GB occupied from the first second, create the image in a dynamic format.

### Extra information
* If you are coming from stock you will need to move all your books from /sdcard/Books/ to sdcard/kindle/ (please note some of these may need to be redownloaded).
* There is some confusion regarding the entries "Phone Idle" and "Cell Standby" in the battery/usage stats. This is referring to the device idle and standby, it could just as easily say "Tablet Idle" and "Device Standby" but it wasn't considered in the original creation (this can be resolved w/ an overlay most likely but I have not researched it). Your device battery is not wasting away to some mysterious phone/cell service so sleep easy.
* By default the lockscreen is disabled; to enable it simply go to 'settings' > 'cyanogenmod settings' > 'tablet tweaks' > uncheck disable lockscreen.
* You will likely want to install an app to supplement a back button; for this there are button savior and another (allows gestures) which I can't remember currently (someone post the name and I will update this lol).
* I would also recommend VolumeControl for supplemented sound controls.
* If for some reason you flash the CWM build in TWRP do NOT select reboot recovery; if you do the device will go into a recovery loop. If you find yourself in such a situation from recovery connect to your computer and from within platform_tools (or wherever you have adb) input 'adb shell idme bootmore 4000'

### Credits and Thanks
* JackpotClavin for origin concept build.
* Whistlestop for the initial repos.
* Sitic/Nind for his work on CM9 (some of which has helped me figure out how to change stuff for us).
* Pkt_Lnt providing fixes.
* SkimpKilla for compiling this into something people can flash (even if it is a bit modified ;) ).
* Takenover83; a custom backup file is the weirdest concept ever but people seem to like it so have at it.
* Hashcode for awesome work on CM9
* The CyanogenMod and AOSP teams
