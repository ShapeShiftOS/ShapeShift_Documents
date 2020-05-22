# Shift_Documentation
XDA Thread Template and Some other stuff

Getting Started
==================================================

<img src="https://i.imgur.com/urc7KCpr.png"> 

How to build ShapeShiftOS ROM for your device - Tutorial
--------

>> To get started with the building process, you'll need to get familiar with [Git and Repo](http://source.android.com/source/using-repo.html).

### Build Environment

- Tested and Working on any version of Ubuntu - 14.04,14.10,15.04,16.04,18.04 (64-bit)
- Any other distribution based of the Ubuntu Distro such as Lubuntu, Xubuntu and etc.
- Any form of Terminal
- Decent hardware (minimum of at least a quad core CPU and 16 GB of RAM)
- A storage unit of any kind (We recommend utilizing SSDs as Mechanical HDDs slow down the build proccess drastically and the minimum storage size is 70GB. Having more will be useful with CCache[More on that later])
- Required Packages should have been installed

### Required Packages [this is for ubuntu 16.04, for other variants it may differ]
##### Simply copy and paste this in a terminal window:
>> [Hint: This command updates the Ubuntu Packages List (Install Listing) and install the required version of Java]

```bash
     $ sudo apt-get install openjdk-8-jdk
```

### Let that install and then proceed.

### More copy and paste:
>> [Hint: Running this command installs the other required packages to build android]

```bash
     $ sudo apt-get update && sudo apt-get install bc git-core gnupg flex bison gperf libsdl1.2-dev libesd0-dev libwxgtk3.0-dev squashfs-tools build-essential zip curl libncurses5-dev zlib1g-dev openjdk-8-jre openjdk-8-jdk pngcrush schedtool libxml2 libxml2-utils xsltproc lzop libc6-dev schedtool g++-multilib lib32z1-dev lib32ncurses5-dev lib32readline6-dev gcc-multilib maven tmux screen w3m ncftp adb fastboot repo python default-jdk
```

### Getting the source
- Making required directories
- Obtaining the repo binary
- Adding repo binary to your path
- Giving the repo binary proper permissions
- Initializing an empty repo
- Syncing the repo

>> Alright, so now we’re getting there. I have outlined the basics of what we’re about to do and broke them down as I know them. This is all pretty much going to be copy/paste so it’ll be fairly difficult to screw this up :)

##### Make directory for the repo binary

```bash
      $ mkdir ~/bin
```

##### Add directory for the repo binary to its path

```bash
      $ PATH=~/bin:$PATH
```

##### Downloading repo binary and placing it in the proper directory

```bash
      $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
```

##### Giving the repo binary the proper permissions

```bash
      $ chmod a+x ~/bin/repo
```

##### Creating directory for where the ShapeShiftOS repo will be stored and synced

```bash
      $ mkdir ~/ssos
      $ cd ~/ssos
```

##### Initializing the ShapeShiftOS repo and downloading the manifest

```bash
      $  repo init --depth=1 -u https://github.com/ShapeShiftOS/android_manifest.git -b android_10
```

##### Syncing the source
>> [Hint: This might take a long time as the source is ~26GB]

```bash
      $  repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

### Building the ShapeShiftOS ROM
- Preparing Required Binaries and Device Drivers
- Setting Up CCache (Optional)
- Building ShapeShiftOS

>> Congratulations on the succesfull build initialization! Now, we shall go ahead and prepare to build for your device!

##### Preparing ShapeShiftOS ROM for devices
- For building ShapeShiftOS for your device, ensure that your trees can successfully build PixelExperience as our code is extremely similar to theirs. Of course, we do require some additional overlays in your tree to make the best out of ShapeShiftOS.

If you have a device with a notch, add this to your overlay's overlay/frameworks/base/core/res/res/values/config.xml:
>>     <!-- Whether device has a physical display cutout -->
>>     <bool name="config_physicalDisplayCutout">true</bool>

If your device has an AMOLED screen, add this to your overlays overlay/frameworks/base/core/res/res/values/config.xml:
>>      <!-- Whether the device supports Smart Pixels -->
>>      <bool name="config_enableSmartPixels">true</bool>    

If your device has an in-display fingerprint scanner, you have to adapt these commits to your device (Only recommended if FOD doesn't work or has problems on your device!)

      https://github.com/ancient-devices/device_oneplus_fajita/commit/8d42520f76e949ce2148d7f59f44d4df425e0d2c
      https://github.com/ancient-devices/device_oneplus_fajita/commit/8220345e1a11ea88e78662c5ef49b906c7a67551
      https://github.com/ancient-devices/device_oneplus_fajita/commit/496222e61eb7aff24116bce27b5cf3c0c6afbced
      https://github.com/ancient-devices/device_oneplus_fajita/commit/cf344bc37865f9deb724672c17dbf8ead02e8883

##### Setting Up CCache
- CCache is a method of utilizing a specified storage space to speed up building. It can be referred to as the same caching your android device does to speed up application and system boot times. In this case, CCache will help build ShapeShiftOS faster than standard build times (Able to cut-down 50% of time taken to build).
- To set up CCache, follow the following:

```bash
        $ echo "export USE_CCACHE=1" >> ~/.bashrc
```

##### To build ShapeShiftOS ROM

```bash
      $ cd ~/exui
      $ source build/envsetup.sh
      $ lunch aosp_<devicecodename>-userdebug
      $ make bacon -j$(nproc --all)
```

##### Obtaining the zip created from the build process
>> To get the zip file that has been built, navigate to the following directory and find for the zip file:

```bash
      $ cd ~/ssos/out/target/product/<devicename>
```

OR

```bash
      $ cd $OUT
```

>> If you found it, then congratulations! If you didn't, try retrying the build process but before doing so, ensure you do the following to make sure your next build is clean;

```bash
      $ cd ~/ssos
      $ make clean
      $ repo sync --force-sync
```

>> After doing so, redo everything stated from the Building Section.

##### For those who successfully built ShapeShiftOS

>> Well, Congratulations on your victory! Now, you have a .zip file that flashable to your device! Share it to the internet as you wish but be sure to contribute back and also give credits to the ShapeShiftOS Team and its contributors! Also keep in mind that if an official build exists for a device, no unofficial builds should be released publicly. Do come and build ShapeShiftOS another time as source code is routinely being improved upon. If you wish to contibute, feel free to make a pull request to the ShapeShiftOS Team! See you again builder! Note: If you decide to create a thread on xda, please limit the thread to only one per device. More than one thread will not be allowed per device as it creates confusion amongst users and makes it hard for us to track bug reports.

-----------------------------------------	
Getting Official Maintainership for ShapeShiftOS
==========================================
>> To get Official Maintainership for ShapeShiftOS you should have a stable device source with all the main components working. Read the [**charter**](https://github.com/ShapeShiftOS/Shift_Documentation/blob/shapeless/Charter.mkdn) to get a clearer idea.

>> First make an unofficial build of ShapeShiftOS and post in [**XDA**](https://xda-developers.com). Make sure you use the template here! Click on the raw button and change the links up where ever required.

>> Then, Ping us on Telegram :- [**ShapeShiftOSChat**](https://t.me/shapeshiftoschat) 

>> Join our [**Telegram Channel**](https://t.me/shapeshiftoschannel) and our  [**Telegram group**](https://t.me/shapeshiftoschat)

>> To publish builds use our Template : [**ShapeShiftOS XDA Template**](https://github.com/ShapeShiftOS/Shift_Documentation/blob/shapeless/Template.txt)

----------------------------
