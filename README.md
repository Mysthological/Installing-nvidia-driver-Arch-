# Installing-nvidia-driver-Arch
How to install proprietary NVIDIA drivers for Arch Linux.

![Arch Linux Logo](https://archlinux.org/static/logos/archlinux-logo-dark-90dpi.ebdee92a15b3.png).

## Default Prerequisites
I'm doing this mostly for installing hyprland with nvidia.Cause you've no other Choice rn.So you need proprietary NVIDIA drivers for Arch Linux.
After finishing base arch installation now we can just start installing nvidia drivers.

## Important links
-[Arch Linux Homepage](https://archlinux.org/ "Arch Linux Homepage")
-[nvidia-all](https://github.com/Frogging-Family/nvidia-all)
-[More about drm-kms](https://www.kernel.org/doc/html/v4.15/gpu/drm-kms.html)

## Step 1: Install required packages and enable multilib
  
  1.open pacman config with vim or nano(your Choice)
    `% vim /etc/pacman.conf`

  2.Then look for the line [multilib] and remove the comments from that line, and the line below it.
  ` #[multilib]
    #Include = /etc/pacman.d/mirrorlist`
    
  3. For god's sake Just udate your system with yay:
    `yay  -Syu`

## clone the repo and run the installer
```
git clone https://github.com/Frogging-Family/nvidia-all.git
cd nvidia-all
makepkg -si
```
## For Identify GPU Architecture
```
  lspci | grep -E "VGA|3D"
```

## DKMS or regular?
DKMS is recommended as it allows for automatic module rebuilding on kernel updates.So just relax and choose the latest nvidia driver(dkms).
  
## Setting the kernel parameter depends on what bootloader you are using. I'm using grub.
  1.Edit the GRUB configuration file:
  ` sudo vim /etc/default/grub `

  2.Find the line with GRUB_CMDLINE_LINUX_DEFAULT Append the word,nvidia-drm.modeset=1 inside the quotes.If you are using Linux kernel 6.11 or newer, you must also add nvidia-drm.fbdev=1.
    `GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet splash nvidia-drm.modeset=1 nvidia-drm.fbdev=1"`
    
  3.Update GRUB:
    `sudo grub-mkconfig -o /boot/grub/grub.cfg`
    
## Early Loading of NVIDIA Modules:

## Enable DRM Kernel Mode Setting.
  1.Edit /etc/mkinitcpio.conf and add nvidia modules:
  `MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)`
  2.Find the line that says HOOKS=().find the word kms inside the parenthesis and remove it.
    `HOOKS=(just remove "kms".Nothing else please!)`
  3.Rebuild the initramfs:
  `sudo mkinitcpio -P`

## Blacklist the Nouveau driver to use only nvidia as default gpu :
  1.Edit the file /etc/modprobe.d/blacklist-nouveau.conf:
   ```
  blacklist nouveau
  options nouveau modeset=0
  ```
 2.Update the configuration:
 ```
  sudo update-initramfs -u
  ```







