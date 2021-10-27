  To Start the install for Arch Linux, we first needed to install an ISO that we could base our VM off of. To do that I downloaded from http://mirror.arizona.edu/archlinux/iso/2021.10.01/
After you install the ISO from the University of Arizona website, you then need to create the VM on VMWare fusion. 
For this step, you have to go into File > New then click "Install from disc or image" and then continue.
From here, I clicked "Use another disc or disc image" and found the iso install file and then clicked continue.
On the configuration page, you are going to click Linux then scroll until you reach "Other Linux 5.x and later kernel 64-bit", and then click on this and continue. 
On the next configuration page, select UEFI
Finally, on the Summary page, click "Customize Settings", go through to Hard disk, and set it to 20 GB instead of 8 GB, then go to  processors and Memory, and set memory up to 2048 MB instead of 768 MB. 
Select confirm and then go to the "Arch Linux install medium (x86_64, UEFI).
First step is to verify the installation is in UEFI mode, by running ls /sys/firware/efi/efivars
From here, youll want to check the network configuration by pinging a website from the terminal using "ping-c 4 www.google.com"
Then youll need to update the system clock using "timedatect! set-ntp true"
From here you check the partitions on the disk by running "lsblk"
From here youll need to partition the disk for the install by running "cfdisk /dev/sda" and then going through the ui and setting up new partitions. 
We have to set up 3 partitions through this UI, the first is through gpt on the label type, start it by selecting New, and typing 500M to create SDA1, then set the type to EFI System
Set up another parition by going to the free space, and setting up the root partition, entering 18.5G for the size.
Create another partition by going to new and then creating the swap partition, through entering 1G on the size, and selecting Linux Swap for the partition swap on the type menu.
Move down on the UI to Write, and then select enter, and then type yes to confirm the partition table, this should create all three partitions. 
From here we need to take the three partitions and make them into the the systems we want, so we start by creating the swap file system by running the command "mkswap /dev/sda3" and then running the command "swapon /dev/sda3"
After we have created the swap system, we now need to create the root system by running the command "mkfs.ext4 /dev/sda2", this creates our root file system. 
After we have created the root file system, we now need to create the EFI file system by running "mkfs.fat -F32 /dev/sda1"
Since we have created the file partitions, we now need to mount them on the partitions so that they can be installed, we first start with the root parititon by running "mount /dev/sda2 /mnt"
We then need to create a boot directory to mount on the EFI partition, by running "mkdir /mnt/boot"
We then need to mount the EFI partition onto the EFI directory by running the "mount /dev/sda1 /mnt/boot"
We then need to install the Arch Linux subsystems by running "pacstrap /mnt base linux linux-firmware", this will install the necessary subsystems after downloading them to the system. 
From here we need to run an fstab file to mount the parititons, by running "genfstab -U /mnt >> /mnt/etc/fstab"
We now need to chroot into the arch system to set up some more details on the linux system, we do this by running "arch-chroot /mnt"
Next we need to set up the timezone by running "ln -sf /usr/share/zoneinfo/US/Central /etc/localtime"
we then can set up the locale gen file by running "pacman -S vim" to install a text editor to edit the gen files
From here we need to run "locale-gen" to generate the locales on the config.
We then need to enter the locale file by running "vim /etc/locale.conf" and then adding "LANG=en_US.UTF-8" to the file so that we have set the linux language config. 
We then need to add the hostname from the /etc/hosts file by running "vim /etc/hosts.conf" and then adding "127.0.0.1 localhost" to the first line, "::1 localhost" in the second line,  and then "127.0.1.1 archvm.localdomain archvm" in the third line. 
We then need to enable the networking for the vm by running "systemct1 enable systemd-networkd" and then "systemct1 enable systemd-resolved" 
We can test to ensure that the network was enabled by running "ip addr" and there should be an lo and an ens33 value here.
Next we need to vim into /etc/systemd/network/20-wired.network and enter "[Match]" on the first line and then "Name=ens33" on the second like, on the fourth "[Network]" and then on the fifth line enter "DHCP=yes".
Exit out of the vim, and then run "passwd" to set the root user password
From here we need to install grub and the efi boot manager by running "pacman -S grub efibootmgr" 
We then also need to install the grub bootloader to the EFI partition by running "grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB"
We then need to generate the grub configuration file by running "grub-mkconfig -o /boot/grub/grub.cfg"
We have then completed the Arch linux installation, and then need to exit by running "exit" and then unmount the system by running "unmount -R /mnt" and then reboot the system by running "reboot"
After this, we'll need to sign in using the root password we set up earlier, and then run "
