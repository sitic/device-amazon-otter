# CM9 for Kindle Fire

## Info

|||
|-----------------------------------:|:--------------------------|
|**Discussion thread**: | http://forum.xda-developers.com/showthread.php?t=1411895
|**Building thread**:   | http://forum.xda-developers.com/showthread.php?p=20844007#post20844007
|**IRC Channel**:   	| #kindlefire-dev on freenode
|**Bug List**:	    	| https://docs.google.com/spreadsheet/ccc?key=0AqxTqi6CPt-RdEtHbFhHNDdjZGVONld1OGFEYmRiWUE#gid=0


## Building 

### Initialize
[setup linux/OS X](http://source.android.com/source/initializing.html) please note: it must be sun-java-6, not openjdk

### Download CM9 sources

```bash
mkdir cm9
cd cm9/
curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > repo
chmod a+x repo
./repo init -u git://github.com/CyanogenMod/android.git -b ics
./repo sync -j16
```

### Download Kindle Fire device tree

```bash
git clone git://github.com/JackpotClavin/Kindle-Fire-CM9.git github
mkdir -p device/amazon/
mkdir -p vendor/amazon/
rsync -a github/device/otter device/amazon/
rsync -a github/vendor/otter vendor/amazon/
./vendor/cm/get-prebuilts
```

### Optional SystemUI

```bash
rm -rf frameworks/base/packages/SystemUI
git clone git://github.com/JackpotClavin/SystemUI-For-CM9-Kindle-Fire.git frameworks/base/packages/SystemUI/SystemUI
```

### Compile

```bash
source build/envsetup.sh
brunch otter -j$(grep -c processor /proc/cpuinfo)
```

this sould produce an falshable out/target/product/otter/cm_otter-ota-eng.$USER.zip file, if the signing process fails try to run it again:

```bash
./device/amazon/otter/releasetools/otter_ota_from_target_files -v \
           -p out/host/linux-x86 \
           -k build/target/product/security/testkey \
           --backup=true \
           --override_device=auto \
           out/target/product/otter/obj/PACKAGING/target_files_intermediates/cm_otter-target_files-eng.$USER.zip out/target/product/otter/cm_otter-ota-eng.$USER.zip
```


###Notes for compiling on Mac OS X (by davidnintendo)
* Xcode 4 may not work. Use Xcode 3 instead. You may need to modify the installer to get it working on Lion, though. Use Google to get instructions. 
* MacPorts didn't work well for me either. Homebrew worked better. 
* If your drive is on a non case-sensitive format, you will need to use a disk image to build. Make sure to have enough space on it to compile because everything including intermediate files will be created in the disk image. 40 GB will be enough. Also, if you don't want those 40 GB occupied from the first second, create the image in a dynamic format.

