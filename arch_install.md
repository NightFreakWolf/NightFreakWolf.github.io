# Arch Linux Install

## Setting up Internet
- dhcpcd
- use ping to test if internet connection is working

## efivars
- modprobe efivarfs

## Update system clock
- timedatectl status

## fdisk
- fdisk -l finds /dev/sda
- cfdisk /dev/sda and select gpt mode because its compatiable with UFEI mode
- Select gpt and create 2 partions in EFI mode(500M) and the larger one in Linux LVM(rest of space).

## Format Partitions
	*I had problems with the original method so I had to use Linux LVM because it was the only method that would load up my GUI and pass through the boot process this fix took hours to find.
- pvcreate --dataalignment 1m /dev/sda2
- vgcreate volgroup0 /dev/sda2
- lvcreate -L 5GB volgroup0 -n lv_root
- lvcreate -l 100%FREE volgroup0 -n lv_home
	*Creates 2 groups in the linux lvm partition*
- modprobe dm_mod
- vgscan
	*checks to see if the commands worked properly*
- vgchange -ay
- mkfs.fat -F32 /dev/sda1
- mkfs.ext4 /dev/volgroup0/lv_root
- mkfs.ext4 /dev/volgroup0/lv_home
	*Formats files to mkfs*

## Mount File Systems
- mount /dev/volgroup0/lv_root /mnt
- mkdir /mnt/home
- mount /dev/volgroup0/lv_home /mnt/home

## Installing packages
- pacstrap -K /mnt base linux linux-firmware
- pacman -S nano openssh base-devel
*bc I enjoy nano* 

## Configuring System
- genfstab -U -p /mnt >> /mnt/etc/fstab
- cat /mnt/etc/fstab 
	*To check the file*
- arch-chroot /mnt
- ln -sf /usr/share/zoneinfo/America/Detroit /etc/localtime
- hwclock --systohc
	*Selecting time zone for the system*
- nano /etc/mkinitcpio.conf
- mkinitcpio -p linux

## Localization
- nano /etc/locale.conf
- Control w to find US_EN and delete #
	*See I'm using it

## Network Config
- pacman -S networkmanager
- systemctl enable NetworkManager
	*Forces it to run Network Manager on startup*

## Root Password
- passwd
	my password for root is *rootpasswd*

## Setting Basic Interface
- pacman -S grub efibootmgr dosfstools os-prober mtools
- mkdir /boot/efi
- mount /dev/sda1 /boot/efi
- lsblk
- grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
- mkdir /boot/EFI
- mount /dev/sda1 /boot/EFI
- cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
- grub-mkconfig -o /boot/grub/grub.cfg
- umount -a
- reboot
	*After restart system should reboot and ask you for your login if it worked*

## Other
### Creating users
- useradd -m -g users -G wheel cameron
- useradd -m -g users -G wheel codi
- passwd codi GraceHopper1906
- passwd cameron rootpasswd
	*I edit the sudoers file to remove the # next to the wheel group*
### GUI
	*I used LXDE as my GUI*
- pacman -S lxde-gtk3
- systemctl start lxdm.service
- systemctl enable lxdm.service
- pacman -S firefox 
	*I also installed wine to be able to install putty to my virtual machine so it can install .exe files*
- pacman -S openssh
	*So I can use ssh on my system to connect to the class gateway.*
