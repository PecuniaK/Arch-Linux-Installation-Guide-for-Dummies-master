# Arch Linux Installation Guide for Dummies

Welcome to Arch Linux installation guide for dummies. I hope this guide will help [penguins](https://en.wikipedia.org/wiki/Tux_(mascot)) (like me) get a working Arch Linux install.

<b>I intend on making this guide as wholesome and descriptive as possible by adding relevant information wherever needed such that it appeals to most users</b>.

## Bringing you upto speed about Arch Linux

### What is Arch Linux & Why do you care?

Put plain-and-simple, Arch Linux is a glorious GNU/Linux distribution you should be using if you believe in Simplicity, Usability, and Fashion. It is a Do-It-Yourself (DIY) distro that makes you decide the best ways to cook yourself an Operating System that suits your needs.

Arch Linux is for advanced/experienced users since it expects you to know your ways around the GNU/Linux systems in general. Arch is far less forgiving on users than any other general-purpose distro out there. 


## Motivation for this guide

The [Official Arch Installation Guide]() is a very clear, conscise, and to the point guide that should be actually good enough for most users to install Arch if they know their ways around things (Terminal, basics of a GNU system, BIOS/UEFI, Partitioning, Network Configuration etc).

And yes, you should be using a fainter/forgiving distro such as Ubuntu/Fedora/Mint instead of Arch Linux if you don't understand those things well enough, but for most people such as myself who need a tad bit of extended help in learning and building an Arch Linux system, this guide is intended for you.

This guide is a fork of [arch linux for dummies](https://github.com/jieverson/dotfiles/wiki/arch-linux-for-dummies), I've added detailed and extended descriptions in places I felt are lacking.


## Note before Installation

Hands-on experience with installing Arch Linux is the best way to understand the concept of "Baking" an operating system for yourself, to suit your needs, and to fit your style. If you have a spare computer or a [Virtual Machine]() to test and learn things as you make your first try at installing Arch, please use it.

# Getting Started (The Pure, real, and KISS way of installing Arch Linux)

## Essential Checklist before beginning installation

<b>1.</b> The desired computer <b>must be</b> of `x86_x64` CPU Architecture (also commonly known as `x64` or `64bit systems`) since Arch Linux only officially supports this architecture. Your CPU is most probably 64bit if it was made in the last decade.

<b>2.</b> Download Arch Linux from [official website](). And prepare your Arch Linux installation media, either with a CD/DVD or USB flash drive (Preferred) [using the official guide from Arch Wiki here]().

<b>3.</b> You must have an active and decent internet connection to be able to download certain essential packages during the installation.

For most cases while preparing USB install, use [Win32DiskImager]() on Windows PC, or `dd` utility on a Linux system, to [put the ISO image onto USB]() drive.

<b>Most Operating Systems'</b> vanilla installation process involves:

<b>1.</b> Setting `Localization` preferences (such as `Region and Language`, `Keyboard Layouts`, `Clock`, `TimeZone` etc).<br>
<b>2.</b> `Network Connectivity` and `Network Configuration`.<br>
<b>3.</b> `Partitioning` Disks, Setting up `FileSystems`, and setting `Encryption`.<br>
<b>4.</b> Installing `System`, setup `Bootloader`.<br>
<b>5.</b> Setting up `Users, Groups, and permissions`.<br>

You'll notice that our installation process will be similar more or less, We follow the below checklist to keep track of the installation progress, this checklist largely resembles the [Official Installlation Guide]() except with the difference that we add more details and descriptions at each level for the mindfulness of newbie users.

## Installation

Plug in and boot into the installation media after which you should be prompted with a root shell indicating that it is ready to accept commands for installation.

**Note:** A typical Arch Linux installation involves creating, and editing of various configuration files. For this purpose, we use any one of the terminal text editor programs such as `nano`, `vi`, `vim`, `emacs`, `ed` etc. The most preffered and easy way is to use `nano` editor, or `vi/vim` if you're familiar with it.

**Note:** The `base` package group offers all the essentials (kernel, core utilities, libraries, APIs, and scripts) needed to get your system up and running, and takes less than 800 MB of disk space. The frequently mentioned `base-devel` package group consists of APIs, Libraries, and binaries (`gcc`, GNU binutils, `make`, `bison`, etc) often used for development, this package group is desired by most developers and users alike, and you should probably install it as described later in the guide.

### Step 1. Set Keyboard Layout (Optional)

This will be the default keyboard layout during the installation process. The default is English US (en.US), if you need to set another layout:

1. List available layouts using the command: `# ls /usr/share/kbd/keymaps/**/*.map.gz`
2. Select preferred layout using the command: `# loadkeys de-latin1` (this example uses German layout).

### Step 2. Verify Boot Mode

Typically there are 2 boot modes --`BIOS` and `UEFI`. Traditional PCs use `BIOS` whereas most modern PCs/Laptops use `UEFI` firmware. `UEFI` is to `BIOS` what a Tree is to its Seed (this analogy over-simplifies the distinction).

Arch Linux needs to know if you're booting from `BIOS` mode or `UEFI` since it influences the installation procedure.

To verify boot mode, enter the command: `# ls /sys/firmware/efi/efivars`

If the output returns blank it means you're booted into `BIOS` mode, else you're in `UEFI` mode.


### Step 3. Test Internet Connection & Set System Clock

An active connection is required to download packages later during the install.

Test your connection using the command: `# ping www.google.com`.

Troubleshoot any connection problems using the [NetworkConfiguration](https://wiki.archlinux.org/index.php/Network_configuration) page. You can also use the `wifi-menu` command to connect to WiFi.

Make sure your system clocks are accurate using the following commands:

```
# timedatectl set-ntp true
# timedatectl status
```

### Step 4. Partitioning Disks, Creating and Mounting File Systems

This whole section is about making room for Arch Linux on your disk and readying it for installation to begin.

For partitioning your disk, we can use multiple tools such as `fdisk`, `parted`, or `cfdisk`. We will be using `cfdisk` command tool for this guide since it simplifies the process.

Firstly get a glance of the structure and layout of your disks by using the command: `# lsblk`

you'll see notations used such as sda, sdb, etc to represent various storage devices found. `sd` stands for storage device/disk and the `a/b/c...` are used to denote each disk cumulatively. Chances are you'll be seeing `sda` and `sdb` in the top-level which can mean that `sda` denotes the internal hard-drive of your computer and `sdb` represents the USB flash drive you are using at the moment as Arch Linux installation media.








<FINSIH_EDITING_FROM_BELOW>











`sda` is the device we shall make changes to in order to make room for Arch Linux installation.  

Lets assume you already have 2 partitions under your hard-drive, they are most likely referred to as `sda1` and `sda2`. It is crucial to understand this notation since you'll be making changes to the disks with/without data of critical importance.

Let's use `cfdisk` tool to partition the disk:

`# cfdisk`

Like most users, we typically create 3 partitions for our installation to reside as described below:

| Partition (Mount Points) | Type | Recommended Size | Description |
| :--- | :---      | :--- | :----- |
| `/` (root partition)   | `ext4` (aka `83 Linux` in `cfdisk`)      | around 15-20 GB of space | Required OS partition
| `swap`     | `swap` (aka `82 Linux Swap` in `cfdisk`) | [Depending on your RAM]()      | Required for [performance]()
| `/home`    | `ext4` (aka `83 Linux` in `cfdisk`)       | The rest of your disk space      | This will be your data/media partition

The `cfdisk` utility is pretty self-explanatory, use the arrow keys to navigate and create desired partitions more or less similar to the above table.





### UEFI mode
This tutorial will consider that you have a [UEFI](https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface) motherboard with UEFI mode enabled. To verify you are booted in UEFI mode, check that the following directory is populated:
```
# ls /sys/firmware/efi/efivars
```
**Note:** _If arch is been installed in a VM, remember to change the motherboard settings to support UEFI/efivars._

### Preparation
Make sure your internet connection is working (it probably should):
```
# ping google.com
```
If no connection is available, see [Network configuration](https://wiki.archlinux.org/index.php/Network_configuration).
You can also use the `wifi-menu` command to connect to WiFi.

Make sure your system clock is accurate:
```
# timedatectl set-ntp true
# timedatectl status
```

### Partitioning
This section is based on [Partitioning](https://wiki.archlinux.org/index.php/Partitioning), [GPT](https://wiki.archlinux.org/index.php/GUID_Partition_Table), [GNU Parted](https://wiki.archlinux.org/index.php/GNU_Parted), [EFI](https://wiki.archlinux.org/index.php/EFI_System_Partition) and [Swap](https://wiki.archlinux.org/index.php/Swap).
Identify the devices where the new system will be installed:
```
# lsblk
```
**!Note:** _In this section, the "sd**xy**" notation will be used, where **x** represents device and **y** represents partition (eg. sd**a1**, sd**a2**, sd**b1**, etc...)._

Let's start partitioning our device (eg. sda, sdb, etc...):
```
# parted /dev/sdx
```
If you run a `print`, you will see the partition label is not defined. Let's set the partition label to gpt:
```
(parted) mklabel gpt
```
If you `print` again, you will now be able to see an empty partition table.
In this tutorial, we are going to create a basic gpt with 3 partitions (first for boot, second for swap, and third for our data). The boot partition can have a size between 260Mb and 512Mb. The swap partition needs to have at least your RAM size (it's recomemnded to have 2x RAM size). The remaning space is going to be allocated to your data. Let's format:
```
(parted) mkpart ESP fat32 1MiB 513MiB
(parted) set 1 boot on
(parted) mkpart primary linux-swap 513M 3G
(parted) mkpart primary ext4 3G 100%
```
You can `print` again to check if your partition table is ok. Exit from parted with:
```
(parted) quit
```

### Formatting
You need to format each of your partitions, except for swap. All available partitions on device can be listed with the following command:
```
# lsblk /dev/sdx
```
Format the boot partition to fat32:
```
# mkfs.fat -F32 /dev/sdx1
```
Set up swap partition:
```
# mkswap /dev/sdx2
# swapon /dev/sdx2
```
Then, format your data partition:
```
# mkfs.ext4 /dev/sdx3
```
If you want, you can check your partitions with `lsblk` again.

### Mount the partitions
Mount the root partition to the _/mnt_ directory of the live system:
```
# mount /dev/sdx3 /mnt
```
Now, mount the boot partition:
```
# mkdir /mnt/boot
# mount /dev/sdx1 /mnt/boot
```

### Installation
Execute the `pacstrap` script to install base packages:
```
# pacstrap /mnt
```
Generate an [fstab](https://wiki.archlinux.org/index.php/Fstab) file, to define how disk partitions should be mounted into the filesystem:
```
# genfstab -U /mnt >> /mnt/etc/fstab
```
The next installation steps needs to be running from a bash inside the new system, so change root to the new system:
```
# arch-chroot /mnt /bin/bash
```
Edit the location file with `nano` (or `vi`, if you want):
```
# nano /etc/locale.gen
```
Uncomment `en_US.UTF-8 UTF-8` in `/etc/locale.gen`, save and quit the file, and generate new location:
```
# locale-gen
```
Now set system locale:
```
# echo LANG=en_US.UTF-8 > /etc/locale.conf
```
Select a time zone:
```
# tzselect
```
Go to `/usr/share/zoneinfo` and find your **Zone**:
```
# cd /usr/share/zoneinfo
# ls
```
Go to your zone folder, and find your **Subzone**:
```
# cd MY_ZONE
# ls
```
Create the symbolic link `/etc/localtime`, to your **Zone** and **Subzone**:
```
ln -s /usr/share/zoneinfo/MY_ZONE/MY_SUBZONE /etc/localtime
```
It is recommended to adjust the time skew, and set the time standard to UTC:
```
# hwclock --systohc --utc
```

### Boot loader
Install the packages `grub` and `efibootmgr`:
```
# pacman -S grub efibootmgr
```
The following steps install the **GRUB UEFI** application to `/boot/EFI/grub`, install its modules to `/boot/grub/x86_64-efi`, and place the bootable _grubx64.efi_ stub in `/boot/EFI/grub`:
```
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
```
**UEFI firmware workaround:** Some UEFI firmware requires that the bootable `.efi` stub have a specific name and be placed in a specific location: `/boot/EFI/boot/bootx64.efi`:
```
# mkdir /boot/EFI/boot
# cp /boot/EFI/grub/grubx64.efi /boot/EFI/boot/bootx64.efi
```
If you have an **Intel CPU**, in addition to installing a boot loader, install the `intel-ucode` package:
```
# pacman -S intel-ucode
```
After installing the `intel-ucode` package, regenerate the GRUB config to activate loading the microcode update:
```
# grub-mkconfig -o /boot/grub/grub.cfg
```

### Network configuration
Set the hostname by adding an entry to /etc/hostname, where **MY_HOSTNAME** is the desired host name:
```
# echo MY_HOSTNAME > /etc/hostname
```

#### old way [deprecated]
Run `ip link` to find your ethernet interface name (it should starts with `en`, eg. `enp0s25`).
```
# ip link
```
When only requiring a single wired connection, enable the dhcpcd service, where **MY_EN** is your ethernet interface:
```
# systemctl enable dhcpcd@MY_EN.service
```
For wireless, install the `iw`, `wpa_supplicant` and `dialog` packages:
```
# pacman -S iw wpa_supplicant dialog
```
Additional [firmware packages](https://wiki.archlinux.org/index.php/Wireless_network_configuration#Installing_driver.2Ffirmware) may also be required.
See [Wireless Management](https://wiki.archlinux.org/index.php/Wireless_network_configuration#Wireless_management) for other available methods.

#### better way
```
# pacman -S networkmanager
# systemctl enable NetworkManager.service
# nmtui-connect
```
### Change root password and reboot
Set the root password with:
```
# passwd
```
Exit from the chroot environment:
```
# exit
```
Reboot into the new system:
```
# reboot
```
**Note:** _Remember to remove the CD (or ISO in case of VM)_

After reboot, log in using the user `root`.

### User management
Install the package `sudo`:
```
# pacman -S sudo
```
Create an user for you, where **MY_USERNAME** is your username:
```
# useradd -m -G wheel MY_USERNAME
# passwd MY_USERNAME
```
To grant your user's group `sudo` access, run `visudo` and uncomment the required line.
```
# export EDITOR=nano && visudo
```
Logout as _root_, and start using your new user.
```
# logout
```

### Install shell: zsh
For this tutorial, I will be using `zsh` as my users shell (Feel free to choose a different one)
```
# sudo pacman -S zsh
# chsh -s /bin/zsh
# logout
```
When you login again, zsh will be running, and you will be asked for a few configurations (you can actually skip it).

### Install various common packages
From here you can install your common packages. I will just install some of mine:
```
$ sudo pacman -S gcc make wget tar tmux
```

### Install X
Xorg is the most popular display server among Linux users. Xorg can be installed with the `xorg-server` package.
Additionally, you can install this packages too: `xorg-server-utils` and `xorg-apps`.
```
$ sudo pacman -S xorg-server xorg-server-utils
```

### Driver installation
First, identify your card:
```
$ lspci | grep -e VGA -e 3D
```
Then install an appropriate driver. You can search the package database for a complete list of open-source video drivers:
```
$ pacman -Ss xf86-video
```

### VirtualBox Settings
**Skip** this if arch is not installed in a VM.
```
$ sudo pacman -S virtualbox-guest-utils
$ sudo modprobe -a vboxguest vboxsf vboxvideo
```

### Window Manager: BSPWM
_bspwm_ is a tiling window manager. Install `bspwm`, `sxhkd`, `xorg-xinit` and a terminal emulator (like `rxvt-unicode`):
```
$ sudo pacman -S bspwm sxhkd xorg-xinit rxvt-unicode
```
We will be getting default config files from the Github.
```
$ mkdir ~/.config
$ echo export XDG_CONFIG_HOME="$HOME/.config" >> ~/.zshrc
$ mkdir ~/.config/bspwm
$ cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/
$ mkdir ~/.config/sxhkd
$ cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/
$ chmod +x ~/.config/bspwm/bspwmrc
```
Look at `~/.config/sxhkd/sxhkdrc` and learn the key bindings. To start bspwm on login, add the following to `~/.xinitrc`
```
exec bspwm
```
