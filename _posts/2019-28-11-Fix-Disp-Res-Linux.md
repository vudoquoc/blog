---
title: Fix Display Resolution in Ubuntu 16.04 by installing NVIDIA Drivers
date: 2019-28-11
tags: [Ubuntu, Drivers]
header: 
  image: "/images/ubuntu.jpg"
---
I just finished building my own PC and installing Ubuntu 16.04 alongside Windows 10. The Windows 10 is located on a SSD 256 GB drive and the Ubuntu 16.04 is located on a 1T HDD partition. In order to install these two operating systems on separate drives, please take a look at my post on How to dual boot Ubuntu and Windows 10 on separated hard drives.

After installing Ubuntu, my screen resolution was stucked at 1024 x 768 eventhough my monitor is 4k and my GPU is GeForce GTX 1660 Ti, which is more than enough to handle a 4k display. Checking `System Settings > Details`, the Graphics card was `llvmpipe` instead of Geforce. I had been searching and following several methods on google and all of them resulted in unable to boot into Ubuntu. I eventually came accross the post of [Dibakar Sutra Dhar](https://medium.com/better-programming/how-i-fixed-my-display-resolution-by-installing-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux-489563052f6c) and thanks god his method worked for me. So here I am describing step-by-step how I fixed my problem based on Dibakar's post.

1. Open Terminal using `Ctrl + Alt + T` and update the system after OS installation 
```bash
$ sudo apt update && sudo apt upgrade 
```
2. Search for appropriate driver of your graphic card and download it from NVIDIA's official website. My GTX 1660 Ti is comptatible with version 418.43.
![a](../images/linux-fix-display-res/nvidia-driver.png)

3. Execute the following commands:
```bash
$ cd Downloads/
$ ls
```
If your NVIDIA .run file is not colored green, you can use:
```bash
$ chmod 777 <your .run file>
```
Now if `ls ` again, the file should turn green. Then continue with:
```bash
$ sudo dpkg --add-architecture i386
$ sudo apt update
$ sudo apt install build-essential libc6:i386
$ sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
$ sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```
Update the kernel `initramfs ` and reboot the system.
```bash
$ sudo update-initramfs -u
$ sudo reboot
```

4. Arcording to Dibakar, we just disabled the current Nouveau kernel display server to install the NVIDIA driver. There are two possibilities happen at this step.
- If after rebooting you do not see the purple screen of ubuntu, it is totally fine (as described by Dibakar). Go ahead and login by using `Ctrl + Alt + F1` to `F12 ` (Mine worked with `F1`), enter you username and password. Then execute this comment:
```bash 
$ sudo telinit 3 
```
- If after rebooting you still be able to login ubuntu normally (as in my case). We will reverse the order of the above case by first, open the terminal and type in `$ sudo telinit 3`. This will log out of the purple screen and bring you to a black screen. Then using `Ctrl + Alt + F1` to log in with your username and password.

5. Voila! it's time to install NVIDIA driver. Using the following commands (I assume that you are still in the Downloads/ folder):
```bash
$ sudo su
$ bash <your driver>.run
``` 
The installation will start and it will ask you several questions:
```bash
- Accept License (mine do not have this step).
- The distribution-provide pre-install script failed! Are you sure you want to continue? -> CONTINUE INSTALLATION.
- Install all NVIDIA 32-bit compatibility libraries? -> YES.
- Would you like to run the nvidia-xconfig utility? -> YES.
```
Banggg! After rebooting my display resolution automatically adjusted to 4k. In `System Settings > Details`, the Graphics was changed to `GeForce GTX 1660 Ti/PCIe/SSE2`, which means the graphic card was used by the computer. And in `System Settings > Display`, there were various resolution for me to choose instead of just 1024 x 768 as before.

Many thanks to Dibakar and good luck with your NVIDIA Drivers!

