# Bashbunny Mark II Reference Guide for starters
This guide is written for the Bashbunny Mark II. Most of the following info is already documented online but on many different places. Make sure to check out the official Hak5 sources and other blogposts as you wish! The descriptions below are not my inventions but just a collection of the information I found most useful.

The guide is written for starters, who have little knowledge on the Bashbunny itself. Later on it requires you to have some knowledge on Linux systems as this is the OS I run on my host and is running on the Bashbunny.
All commands used in this Reference Guide is aimed at users who run a Linux distribution (in my case an Ubuntu LTS distribution). You'll probably be able to translate this to other OS's yourself if you feel more comfortable to those.

## Bashbunny Model
The Reference guide is written for the Bashbunny Mark II . You can recognise this by the micro-SD slot and the red-sliding button on the side.
The Mark I does _not have a micro-SD slot_ and _no red button_.

## Switches Bashbunny
The Bashbunny is a USB stick running a Linux distribution. It can function as USB mass storage device, but also as a Linux host or Ethernet port. The sliding button changes the mode your Bashbunny is in. The modes you can run the Bashbunny in are called as follows:

_Face the Bashbunny such that the LED points towards you and the USB connecter points to the right_

The modes:
1. **Arming mode**:
   1. Shift the button all the way to the right
   2. The Bashbunny is now a regular Mass Storage Device which can be mounted as any other regular USB stick. The folder structure however is vital to the functioning of the Bashbunny. So do not modify or remove those as it may impact the functioning of your Bashbunny.
2. **Switch 2 Payload**:
   1. Shift the button to the middle
   2. The Bashbunny will automatically execute the payload as instructed in the file ```payload.txt``` in the ```switch2``` folder. (Refer to section [Payloads](#payloads-op-de-bashbunny-zetten) for more info)
3. **Switch 1 Payload**:
   1. Shift the button to the left
   2. Similar to the Switch 2 Payload mode, except the Bashbunny will execute ```payload.txt``` of the ```switch1``` folder instead of the other folder..


## First Start Guide
After opening the Bashbunny package, you want to get started immediately. As already explained, the bashbunny is both a Linux system as a Mass Storage device. When using the Bashbunny for the first time, you can just handle it as if it is a regular USB mass storage device.
If you want to know how to get shell-access, refer to section [Starting guide for shell access](#starting-guide-for-shell-access).

For your first start, using the Bashbunny as USB mass storage device is however sufficient.

So, remove your Bashbunny from its package, verify if it is the Mark II model and activate the **Arming Mode** by following the instructions in [Switches Bashbunny](#switches-bashbunny).

### The folder structure on your Bashbunny
Insert your Bashbunny into your host. Mount it as USB mass storage device and look at its folder structure. It must look like something as follows:
```
├── config.txt
├── docs
│   ├── EULA
│   ├── full_documentation.html
│   ├── LICENSE
│   └── readme.txt
├── languages
├── loot
├── payloads
│   ├── extensions
│   │   ├── cucumber.sh
│   │   ├── debug.sh
│   │   ├── ducky_lang.sh
│   │   ├── get2_dhclient.sh
│   │   ├── get.sh
│   │   ├── mac_happy.sh
│   │   ├── requiretool.sh
│   │   ├── runpayload.sh
│   │   ├── run.sh
│   │   ├── setkb.sh
│   │   ├── waiteject.sh
│   │   ├── wait_for_notpresent.sh
│   │   ├── wait_for_present.sh
│   │   └── wait.sh
│   ├── library
│   │   └── get_payloads.html
│   ├── switch1
│   │   └── payload.txt
│   └── switch2
│       └── payload.txt
├── System Volume Information
│   ├── IndexerVolumeGuid
│   └── WPSettings.dat
├── tools
├── upgrade.html
├── version.txt
└── win7-win8-cdc-acm.inf

```

### Verify Firmware version
Check the Firmware version and make sure to update it if there is a newer version available. The easiest way to verify its Firmware version is by looking into the file ```version.txt``` in the root folder of the mass storage device. At the time of writing (January 2024) its contents was:
```
1.7_332
```
This means the Bashbunny is running the firmware version **version 1.7-stable**.

### Upgrade Firmware Bashbunny 
If you need to upgrade the Firmware (which is always good if you are not running the latest stable version), go to: https://downloads.hak5.org/bunny/mk2 . Select the newest Firmware version and follow the following steps:
1. Download de tar.gz file (do **not** decompress / extract this file, just leave it)
2. Verify the SHA-256 hash of the file (small differences in the file can brick your device)
3. Make sure your Bashbunny is in **arming mode**, plug it in your host and mount it as USB mass storage device
4. Copy the tar.gz file to the root folder of the USB mass storage device
5. **Safely remove** your Bashbunny from your computer (this makes sure no open file handles etc are still active)
6. Plug your Bashbunny back into your computer while in Arming mode and wait approx. 10 minutes. The Bashbunny will (for firmware 1.1 and higher) flash blue/red while it is flashing the firmware.
7. As soon as the Bashbunny flashes **blue** , the Firmware upgrade is complete!

## Install your first Payload on the Bashbunny
You can run "Payloads" on your Bashbunny (which is probably why you have a Bashbunny anyway). If you switch your Bashbunny into one of the two attack modes, its correlated payload will be automatically executed. One popular
payload is called _QuickCreds_, which allows the Bashbunny to capure Net-NTLM hashes of its target for password-stealing. I will use the `QuickCreds` attack as an example for guiding you through the process of installing a payload
on the Bashbunny.

#### How a payload works on Bashbunny
The Bashbunny also runs a Linux distribution (debian 8 by default). If you switch the Bashbunny to the Switch 1 or Switch 2 mode, your Bashbunny will just automatically execute a bash-script of a certain structure.
When the Bash script is executed, it probably depends on other software libraries. Therefore, a Bashbunny Payload likely consists of two components:
1. ```payload.txt``` : a Bash script
2. A dedicated directory inside the ```/tools``` directory.

The ```payload.txt``` is the driver of your attack, and all the files inside the dedicated directory contains the software libraries your attack depends on.

### Generic Payload Info
You can always write your own payloads using a bash-like language. Hak5 also hosts a GitHub repository with a lot of payloads:
https://github.com/hak5/bashbunny-payloads/tree/master/payloads

Just select any of the interesting payloads and follow its readme or instructions on the internet (carefully) to install it.

#### Easy install of a Payload with a .deb file
Sometimes you can install a payload on the Bashbunny by downloading a ```.deb```-file, copy it to the USB mass storage device into the tools (```Bashbunny/tools/```) folder and re-mount the device such that it will automatically installs the payload.
See https://forums.hak5.org/topic/40971-info-tools/ for some of these ```.deb``` files. 

The ```.deb``` file is just an archive (like .zip files) which is executed automatically by the Bashbunny next time the Bashbunny is launched into Arming Mode. 
During execution, the Bashbunny extracts files into the ```tools``` directory and possibly installs some software libraries if present in the ```.deb``` file. 
After the Bashbunny is done flashing purple, all dependencies for the payload should have been installed. 

Sometimes, the debfile is unpacked with errors. When just looking at the USB and remounting it, there is no way to tell if the debian file was unpacked succesfully.
To make sure everything works, you can also just call the debian file through your shell (as explained in [Starting guide for shell access](#starting-guide-for-shell-access) :

```
root@bunny:~/udisk# ./responder-bunny.deb
```

Now you can directly tell from the shell if the debian file was extracted without errors.


**NOTE: Only using the deb file is not enough! You still need to add the ```payload.txt``` file in one of the switch directories.** . 

Place this ```payload.txt``` either in the switch1 or switch2 folder. Obviously, if you place the ```payload.txt``` in ```/BashBunny/payloads/switch1``` your tool executes when you switch the Bashbunny button into the **Switch 1 Payload** position.

#### Manual Payload installation
If there is no ```.deb``` file available for the payload you are interested in, you can copy complete directories into the ```tools``` folder yourself.
This means you have to place the bash-like scripts in the right directory and add all dependencies like python scripts or other software libraries in the libary folder.

It is more manual labour, but you'll also have much more control on what kind of scripts (and versions) and software is loaded onto the Bashbunny.

To understand exactly what files and dependencies you need to copy to your bashbunny, start with reading the README of the tool and/or the ```payload.txt``` file to understand what your tool tries to execute when launched.
This guideline will follow the manual approach because it also learns you how to get more recent updated libraries etc. in your attack.

As an example, the QuickCreds tool can be installed manually by following these steps: 

### Step 1: Collect the libraries
Looking at the QuickCreds readme, it mentions under _Requirements_ the following:
```commandline
Responder must be in /tools/responder/
```
There is however nothing mentioned where or how to get the Responder. After reading some info online about the QuickCreds payload and Responder, it is quickly understood the Responder is a Python module. The most frequently used version of the Responder **on the Bashbunny** refers to: https://github.com/SpiderLabs/Responder . 
The more recently updated version of the Responder however is available on: https://github.com/lgandx/Responder . It is up to the user to decide which one is needed. 

Download the code-base of the repository you need as a ZIP-file on your host.

### Step 2: Save the libraries onto the Bashbunny
Next step is to move the library (the downloaded code-base) to the right location on the Bashbunny. In case of the ```QuickCreds``` payload, the readme clearly states the library needs to be stored in ```/tools/responder/``` . If this requirement is not clearly documented, look into the ```payload.txt``` to see any suggestion of what to store where.

In case of the ```QuickCreds``` payload for example, the ```payload.txt``` contains the following code snippets:
```
# Set LED yellow, run attack
LED ATTACK
cd /tools/responder

# Clean logs directory
rm logs/*

# Run Responder with specified options
python Responder.py -I usb0 $RESPONDER_OPTIONS &
```

So it is learned that the ```Responder.py``` is executed from the ```tools/responder``` directory. Therefore, the ```Responder``` code-base must be saved into the same location.

So perform the following:
1. Mount the Bashbunny as USB storage device
2. Move the zip-file of the Responder code-base to the USB storage device (from your host, assuming the Mass Storage device is named BashBunny):
```commandline
cp ~/Downloads/Responder-master.zip /media/${USER}/BashBunny/
```
3. Extract the zip-file:
```commandline
unzip /media/${USER}/BashBunny/Responder-master.zip -d /media/${USER}/BashBunny/tools/
```
4. Rename the directory name:
``` commandline
mv /media/${USER}/BashBunny/tools/Responder-master /media/${USER}/BashBunny/tools/responder
```
Now make sure to check out the file ```Responder.py``` to ensure the python script does not need additional libraries/dependencies. 
You can also verify this quickly by running the ```Responder.py``` directly through a shell on the Bashbunny and see if it executes without errors.

**Note: After unmounting and mounting the USB mass storage device, the libraries in the tools folder will disappear because they are moved from the USB partition to the actual OS filesystem. This is totally intended behaviour and do not worry.**

## Starting guide for shell access
While the bashbunny can operate as a Mass storage device, it also runs its own 
lightweight OS (a linux distribution). If you want to interact with the bashbunny 
through a shell on the Bashbunny, you can perform the following steps:


### Using screen to connect with the bashbunny
Connect with the TTY of the USB through the ```screen``` command. The ```screen``` command provides you with a shell through serial ports. This means in the case of the Bashbunny you'll get a shell through the serial connection of USB. This is why you need to provide the serial bus when calling the ```screen``` command and the baudrate your serialbus operates with.

The baudrate for USB is usually ```115200``` , so a safe guess if you are clueless. 

Connecting with the Bashbunny using screen on your Linux host can be done as follows:

```commandline
screen /dev/ttyACM0 115200
```
**You may need to use sudo with this command.**

When entering this command, your terminal will present a shell running on your Bashbunny. 
The default credentials for logging in on your Bashbunny are:

### Default credentials bashbunny
```
user: root
password: hak5bunny
```

After succesfully logging in, you'll see the following on your terminal:
```
bunny login: root
Password:
Linux bunny 3.4.39 #19 SMP PREEMPT Fri Jan 29 09:19:02 UTC 2021 armv7l
           _____  _____  _____  _____     _____  _____  _____  _____  __ __
 (\___/)  | __  ||  _  ||   __||  |  |   | __  ||  |  ||   | ||   | ||  |  |
 (='.'=)  | __ -||     ||__   ||     |   | __ -||  |  || | | || | | ||_   _|
 (")_(")  |_____||__|__||_____||__|__|   |_____||_____||_|___||_|___|  |_|
 Bash Bunny by Hak5     USB Attack/Automation Platform

```
Now you can explore the files in the root homedirectory, when you use ```ls``` in this directory the following two files appear:
```commandline
root@bunny:~# ls
udisk  version.txt
```

The folder ```udisk``` is empty. This is because the physical storage of the bashbunny is divided in 2 partitions. 
One partition is used for the OS (running debian 8 by default at the time of writing in 2024) and the other is just
a blockdevice that can be used as USB mass storage device. 
Like every mass storage device, you need to mount the partition on the Bashbunny OS to be able to navigate through the 
filesystem of that mass storage device.

On the bashunny you can find the block device under ```/dev/nandf``` and you can mount it with your bashbunny shell as follows: 
```commandline
root@bunny:~# mount -o sync /dev/nandf /root/udisk
```
Now you should see the same files under ```/root/udisk``` as you would see when you mount the Bashbunny as a USB storage device on your host:
```commandline
root@bunny:~# ls /root/udisk
'System Volume Information'   languages   tools          win7-win8-cdc-acm.inf
 config.txt                   loot        upgrade.html
 docs                         payloads    version.txt

```

From here you have complete read/write/execute access to your bashbunny! This gives you much more control, and is very helpful when debugging certain payloads you want to run.

Remember you can add software and ```.deb``` files in the ```tools``` folder on the USB mass storage device? They disappear after unmounting and mounting the bashbunny. This is because the OS moves the files from this folder to the ```/tools``` folder in the **system root directory**. When you connect with the bashbunny through screen, make sure to go 1 directory higher with the ```cd ..``` command which directs you to the system root directory. In here (above many other directories) you'll find a ```tools``` directory as well. This directory will actually contain the payloads you have added thorugh the USB mass storage device. 

# Debugging payloads
Lets go back to the example of installing the ```QuickCreds``` payload. Suppose you installed the tool as described 

# Using the Bashbunny in attack mode
When you have added a ```payload.txt``` into one of the switch folders (like ```/Bashbunny/payloads/switch1```) and installed all dependencies either
manually or with the ```.deb``` file your BashBunny should be armed with an attack. To launch the attack, safely eject the USB mass storage device from your host if mounted and switch the button to Switch 1 or Switch 2.

When you changed the position of the switch, plug the BashBunny into your target device (the one you want to attack). Now the BashBunny may flash different LED colors depending on the instructions in the ```payload.txt``` . 
As soon as you are confident the attack has finished, remove the Bashbunny and put it into **Arming Mode** again. 
Mount the BashBunny into your host and check out the folders to see if you have extracted the information your payload was instructed to.

Usually the information is stored in a file inside the ```/Bashbunny/loot/``` folder. But again, 
check out the ```payload.txt``` to make sure where you should find the loot.

# WIP WIP WIP WIP
## Going pro? (or just... dumb?) upgrade debian 8 (jessie) to debian 11 (bullseye)
At the moment Debian 8 (codename: jessie) is currently shipped with the Bashbunny most up to date firmware. This Debian version has already been end of life for some time. This causes a lot of trouble if you want to upgrade python2 to python3 for example (python2 is default in jessie, but now we prefer python3 right...? right?) . Besides python versions, anything you want to install with ```apt``` causes you a lot of trouble when your current distro is EOL. Of course there are workarounds to fix the ```apt``` but why not try to upgrade your linux distro on the Bashbunny? 

**Careful! Hak5 does not recommend this and this may brick your device. Make sure you know what you are doing or how to restore your Bashbunny!** 

## Step 1: Connect the bashbunny with the internet
This step is already described in official documentations. If you know how to do this, skip this step.

To run ```apt update``` you typically need to be able to fetch the APT repos which usually come from the internet. Therefore it is nice to connect your Bunny with the internet to make things a lot easier. Fortunately your Bunny can act as an Ethernet device to get access to the internet so follow these steps below:

### Update your payload
Start by changing one of the ```payload.txt``` files. Remember, if you change the ```payload.txt``` in the ```switch1``` folder, your bashbunny should later be armed in the position for the ```switch1``` payload (see [Switches Bashbunny](#switches-bashbunny) ).
your ```payload.txt``` file should look as follows (note: this attackmode is for Linux hosts. Use another tutorial if you have a different OS) :
```commandline
LED B SLOW
# ATTACKMODE SERIAL STORAGE
ATTACKMODE ECM_ETHERNET
```
This enables the bashbunny to behave as an ethernet port.

### Run a bashbunny-connect script for Internet Access
**All commands in this section must be executed from your host linux computer which has a connection to the Internet**

Next step is to forward the internet connection from your host to your Bashbunny. This requires in-depth knowledge on computer-networks but fortunately the developers of the Bashbunny have also created a script for you to route your internet connection to the Bashbunny.
Download the shell script from: http://www.bashbunny.com/bb.sh like so:
```commandline
$ wget bashbunny.com/bb.sh
```
(Check the shell script if you want to make sure it is not shady)

Now execute the script as root. Don't forget to give the file execution rights (with ```chmod +x bb.sh``` for example). If you have run this script already once, it should look as follows:

```commandline
$ sudo ./bb.sh
           _____  _____  _____  _____     _____  _____  _____  _____  __ __ 
 (\___/)  | __  ||  _  ||   __||  |  |   | __  ||  |  ||   | ||   | ||  |  |
 (='.'=)  | __ -||     ||__   ||     |   | __ -||  |  || | | || | | ||_   _|
 (")_(")  |_____||__|__||_____||__|__|   |_____||_____||_|___||_|___|  |_|  
 Bash Bunny by Hak5     USB Attack/Automation Platform                      
 v1

    Saved Settings: Share Internet connection from wlp59s0
    to Bash Bunny at enx001122334455 through default gateway 172.17.221.1

    [C]onnect using saved settings
    [G]uided setup (recommended)
    [M]anual setup
    [A]dvanced IP settings
    [Q]uit
```
If it is the first time you run this script, it shows an additional text recommended to follow the Guided setup.
I also recommend following the Guided Setup.

**Some tips when running this shell script**
1. **Switch to your payload-mode:** Somewhere in the setup the script will look for your Bashbunny. If your bashbunny is still in Arming mode, safely remove your Bashbunny, switch to the payload which you have modified in [Update your payload](#update-your-payload) and plug the Bashbunny back into your host. Now the ```bb.sh``` script will detect your Bashbunny during the setup. If it does not, your Bashbunny is either not detected, in the wrong mode or your ```payload.txt``` is wrong.
2. Save your settings after setup
3. Run the script as root. If you don't trust the script, read the contents of ```bb.sh``` first (this is good practice anyway).

### Step 3: Register the Bashbunny as Ethernet device
TODO from here
