# Arch Linux Install

Simple overview of use/purpose.

## Description

An in-depth paragraph about your project and overview of use.

## Getting Started

### Creating the Arch VM

* Download the Arch ISO file: https://archlinux.org/download/
* In VMWare Workstation Pro, create a new virtual machine
* Go through the prompted steps and reference the ISO image
* Use "other Linux 5.x or later - x64" as OS selection
* Continue through the prompted steps and create the VM
* The finished VM should have about 20 GB of space and at least 2GB of RAM
* In the VMs files, open the .VMX file and add firmware="efi" (see https://forums.ivanti.com/s/article/How-to-enable-UEFI-in-VMWare-Workstation?language=en_US)
* Start the VM

## First steps in the Arch Environment

### First commands

* Check bootmode: 
        
        ls /sys/firmware/efi/efivars
        
    Notes about bootmode:
    - If it returns with no errors, then you are in UEFI Mode (which is correct)
    - If it says the directory doesn't exist, then you booted in BIOS (which means your .VMX file           change wasnt done correctly)

* Check Internet Connection:
   
         ping archlinux.org
    
    - If the ping works, you are connected
    - If not, verify wireless connection in iwctl (use "help" command for iwctl commands)

* Check time and date:

    - Set:
    
          timedatectl set-ntp true
     
    - Check:
 
           timedatectl status

## Partitioning

* list disks (for our purposes, ignore disks ending in rom, loop, or airloop)

         fdisk -l
         
* enter disk partitioning tool:

         fdisk /dev/sda
         
         
    - Note: During my install, the disk was called /dev/sda. It is likely to be the same.

   - To create /dev/sda1 partition:
      - type the letter 'n' to create new partiiton
      - select primary partition type and partition number 1
      - start of partition: 2048
      - end of partition: +512M
      - type t to change partition type 
      - change partition type to "EFI FAT-12/16/32" ("ef" is the key)
   - To create /dev/sda2 partition:
      - Select primary partition type and partition number 2
      - Use rest of the disk (so default start and end)
      - This for the Arch OS itself
   - type w to write the changes

### File System selection

* There are multiple types of file systems, but I went with FAT32 for sda1 and ext4 for sda2 (which to my understanding is pretty generic
* Assign a file system for each partition using:

```
mkfs.fat -F32 /dev/sda1
```
```
mkfs.ext4 /dev/sda2
```

* check partitions using:
```
lsblk -f
```

### Mounting
* You have to mount the primary system to the disk (currently it is on the ISO)
* In this case we will mount the /dev/sda2 to /mnt using:

```
mount /dev/sda2 /mnt
```

## System Installation
### Install Linux Kernel and Firmware
(this also installs nano and vim)
```
pacstrap /mnt base linux linux-firmware nano vim
```
### Generate fstab file 

```
genfstab -U /mnt >> /mnt/etc/fstab
```
* Use the following command to check it for errors:
```
nano /mnt/etc/fstab
```

### Change root of the system
```
arch-chroot /mnt
```

### Verify Time Zone

* Check system date and time
```
timedatectl
```

* To change it to central time:
```
ln -sf /usr/share/zoneinfo/America/Central /etc/localtime
```
* Generate /etc/adjtime (assumes hardware clock is set to UTC)
```
hwclock --systohc 
```

### Localization
* edit /etc/locale.gen file and uncomment en_US.UTF-8 UTF-8
```
nano /etc/locale.gen
```
* generate locales
```
locale-gen
```
* create /etc/locale.conf file and add LANG=en_US.UTF-8
```
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
```

## Network Configuration
* create /etc/hostname and add a hostname
```
nano /etc/hostname
```
* edit /etc/hosts
```
nano /etc/hosts
``` 
add 
```
127.0.0.1 localhost
::1 localhost
127.0.1.1 yourhostname
```
* install net tools
```
pacman -S net-tools
```

## Random tidbits and bootloader install
* change root password
```
passwd
```
* Download a bootloader (I used grub)
```
pacman -S grub efibootmgr
```
* make directory where EFI partition will be mounted
```
mkdir /boot/efi
```
* mount sda1 to the boot directory
```
mount/dev/sda1 /boot/efi
```
* install grub
```
grub-install --target=x86)64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
```
```
grub-mkconfig -o /boot/grub/grub.cfg
```


