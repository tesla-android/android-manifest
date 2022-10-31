# Manifest for Tesla Android

This version is based on Android 12L (Tiramisu), GloDroid version 0.7.1 (https://github.com/GloDroid/glodroid_manifest)

Please refer to https://tesla-android.gapinski.eu for release notes, hardware requirements and the install guide.

#### Please consider supporting the project: 

[Donations](https://tesla-android.gapinski.eu/donations)

### Supported platforms:
- Raspberry Pi 4b

## Fetching Android sources
- Installing 'repo' tool:
```bash
sudo apt-get install -y python-is-python3 wget
wget -P ~/bin 
http://commondatastorage.googleapis.com/git-repo-downloads/repo
chmod a+x ~/bin/repo
```

- Cloning the source
```bash
mkdir -p TeslaAndroid
cd TeslaAndroid
repo init -u https://github.com/tesla-android/android-manifest
repo sync -cq
```

## You should install additional packages in order to build Tesla Android
(Ubuntu 20.04 LTS is only supported. Building on other distributions can 
be done using docker)

<br/>

- [Install AOSP required 
packages](https://source.android.com/setup/build/initializing).
```bash
sudo apt-get install -y git-core gnupg flex bison build-essential zip curl 
zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev 
x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils 
xsltproc unzip fontconfig
```

<br/>

- Install additional packages
```bash
sudo apt-get install -y swig libssl-dev flex bison device-tree-compiler 
mtools git gettext libncurses5 libgmp-dev libmpc-dev cpio rsync dosfstools 
kmod gdisk lz4
```

<br/>

- Install additional packages (for building mesa3d, libcamera and other 
meson-based components)
```bash
sudo apt-get install -y python3-pip pkg-config python3-dev ninja-build
sudo pip3 install mako meson jinja2 ply pyyaml
```

### Building Tesla Android
```bash
cd TeslaAndroid
source ./build/envsetup.sh
lunch rpi4-userdebug
# After that you have to select your device from the list
make images
```
  
## Deploying TeslaAndroid

After successful build you should see **images.tar.gz** in product output 
folder: 
(*out/target/product/<name\>/images.tar.gz*)  
  
### Content of archive:
* Utilities: **adb**, **fastboot**. **mke2fs**  
* Partition images: **bootloader-sd.img**, **bootloader-emmc.img**, 
**env.img**, **boot.img**, **boot_dtbo.img**, **super.img**  
* Recovery GPT image: **deploy-gpt.img**  
* Recovery sdcard images: **deploy-sd.img**, **deploy-sd-for-emmc.img**  
* Scripts: **flash-sd.sh**, **flash-emmc.sh**  
  
### Step 1
Using any available iso-to-usb utility prepare recovery SDCARD.  
In case you want to flash Android on sdcard, use *deploy-sd.img*  
  
### Step 2
Insert recovery sdcard into the target board.  
Connect microusb cable to OTG connector and your PC.  
Power-up the board.  
  
### Step 3
Ensure you have installed adb package: ```$ sudo apt install adb``` 
(required to setup udev rules)  
Run .*/flash-sd.sh* utility for flashing Android to sdcard
  
*After several minutes flashing should complete and Android should boot*  
  
#### NOTE: Monitor has to be connected to the board and powered-up during 
flashing!
  
