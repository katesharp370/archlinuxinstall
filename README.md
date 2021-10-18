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

* To view different keyboard mappings (default is US, change if needed): 
 
      ls /usr/share/kbd/keymaps/**/*.map.gz

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

         fdisk /dev/disk_name
         
         
    - Note: During my install, the disk was called /dev/sda. It is likely to be similar if not the               same

   - To create /dev/sda1 partition:
      - select primary partition type and partition number 1
      - start of partition: 2048
      - end of partition: +488281K (this is equivalent to a 500MB partition)
      - change partition type to UEFI
   - To create /dev/sda2 partition:
      - Select primary partition type and partition number 2
      - Use rest of the disk (so default start and end)
      - This for the Arch OS itself
      

* How to run the program
* Step-by-step bullets
```
code blocks for commands
```

## Help

Any advise for common problems or issues.
```
command to run if program contains helper info
```

## Authors

Contributors names and contact info

ex. Dominique Pizzie  
ex. [@DomPizzie](https://twitter.com/dompizzie)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release

## License

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Inspiration, code snippets, etc.
* [awesome-readme](https://github.com/matiassingers/awesome-readme)
* [PurpleBooth](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)
* [dbader](https://github.com/dbader/readme-template)
* [zenorocha](https://gist.github.com/zenorocha/4526327)
* [fvcproductions](https://gist.github.com/fvcproductions/1bfc2d4aecb01a834b46)
