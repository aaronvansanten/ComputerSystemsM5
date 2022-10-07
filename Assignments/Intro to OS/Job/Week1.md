# Assignment OS introduction 

Job Logmans – s2331179 

## Assignment 1: *Read about the Raspberry Pi*

I have thought of two things that I personally would use the Raspberry Pi for once we are done with the module. First, I would use it as a high-uptime addition to my home server on which I run a reverse proxy, DNS server, and VPN server. The Raspberry Pi is very suitable for this as it draws very little power and has a very small form factor without a fan. The Raspberry Pi has plenty of horsepower for this use case. I might also host some other services which are light-weight, but still require high up-time. 

Secondly, the Raspberry Pi could be used to make an all-in-one sensor for home automation. Using the GPIO pins and USB ports one could make a presence detection device which can also measure the environment regarding air quality, temperature and more. The Raspberry Pi is quite possibly powerful enough to also run Home Assistant to define and use these automations. 

## Assignment 2: *Get a Raspberry Pi 4 with some accessories*

The kit seems complete. The case was obstructing the SD-card reader a bit, but this seems to be fixed after some tinkering. 

## Assignment 3: *Install a Linux environment on your Laptop computer as well*

I run Pop!_OS by System76 and I`m currently on version 22.04 using a btrfs filesystem. 

## Assignment 4: *Download Raspberry Pi OS and prepare your SD card*

I have installed the 64-bit Raspberry Pi OS and I am able to SSH into the RPi. I have also configured the Pi to connect to my laptop`s Wi-Fi hotspot. 

## Assignment 5: *Prepare the network, boot the Pi and login for the first time*

I have configured the Pi to connect to my laptop`s Wi-Fi hotspot and my laptop is able to simultaneously connect to a Wi-Fi network and to host a hotspot. Therefore, I do not need to configure network sharing over ethernet. This is also much easier for me as I do not have a full-sized ethernet port on my laptop and would need to bring an adapter. 

I am able to SSH into the Pi and the Pi can reach the internet. 

## Assignment 6: *Get used to the shell on your Pi*

1. How can you search through the last commands you executed for a specific one with a certain keyword?

By pressing ctrl+r you can search through your past commands.

2. How can you rename a directory?

By using `mv <old_directory_name> <new_directory_name>`.

3. How can you easily print the number from 1 to 10?

By running `seq 10`.

4. How can you run a specific command 10 times?

By using a structure like `for i in {1..10}; do echo "test"; done`, where the echo command can be replaced with anything.

5. How can you redirect the output of a command to a file?

By appending `>> <filename>` to the command.

6. How can you find all manual pages that contain a specific keyword?

By running `man -k <search-term>`.

7. How can you measure the time it takes to execute a certain command?

By prepending the `time` command to the command. This will execute various measured times.

## Assignment 7: *Get used to reading manual pages*

According to the manual page, a system call is a function which wraps kernel operations while a library call includes all library functions exluding the system calls. A system call is used to call specific system wrapper functions while a library call accesses functions from a library.

The different man pages for printf can be accessed with `man 1 printf` and `man 3 printf`. The first one is clearly described to be used in the shell, while the second one is used and described as a library call. One could also say that the first one is a bash command, while the second one is a C function.

## Assignment 8: *Get used to sudo*

When using sudo the commands gain access to core system functionality. This includes erasing the operating system (until it removes the removing utility). By not granting commands access to sudo this damage can be stopped. However, sometimes core packages need to be updated or added to protected folders. Therefore it is reasonable to use when, for example, updating your packages. It is also quite common to limit hardware access to the sudo user, so it would not surprise me if access to the GPIO pins is limited to the root user.

## Assignment 9: *Update the software on the Raspberry Pi*

`sudo apt update`, then `sudo apt install raspberrypi-kernel-headers openjdk-11-jdk-headless zip iftop atop iotop nmap netcat vim screen libncurses5-dev build-essential fakeroot rsync git flex bison libssl-dev`, then `sudo apt upgrade`.

## Assignment 10: *LEDs*

The red LED (indicated with the PWR label) shows whether the Pi is powered on. The green LED is indicated with ACT and is used for various activities and can be configured.

## Assignment 11: *Prepare to copy files from your computer to the Pi and vice versa*

I used `scp intro-files-2022.zip pi@pi-logmans.local:introfiles` and then the command `unzip introfiles`.

## Assignment 12: *Find a convenient way for you to edit files on the Pi*

I use VSCode and the Remote SSH extension.

## Assignment 13: *Backup*
 
I made a complete backup of the SD card using the 'disks' utility of Ubuntu. This way I can reflash the entire SD card if needed. However, this method does take 32 GBytes of storage on my laptop.

## Assignment 14: *Compile a simple C program on your Pi and your laptop and run it*

`gcc HelloWorld.c -o HelloWorld` and then `./HelloWorld`.

## **TODO** Assignment 15: *Get familiar with C*

## Assignment 16: *Syscalls*

The first difference noticed between my laptop and the Pi is that my laptop has a number of `arch_prctl` calls which are specific to the x86-64 architecture. Therefore the Pi (which runs on ARM) should not have these calls. On my laptop the arch_prctl is followed by a memory map command `mmap` after which the `access` call is made. Then the Pi and laptop make a very similar call with `access` and `faccessat` (they differ mainly in how the filepath is treated, but have the same purpose). There are several other differences, but the biggest one is that my laptop uses a lot more `pread64` calls, where the Pi uses none. It seems that my laptop has much more memory operations and more support/implementation for multithreaded applications as can be seen from the manual pages of these calls.

**TODO - see subquestions**

## **TODO** Assignment 17: *Monitor processes on the Pi*

## **TODO** Assignment 18: *Address space*

## **TODO** Assignment 19: *Stack layout*

## **TODO** Assignment 20: *BenchMem*

## **TODO** Assignment 21: *Creating Processes using Fork*

## **TODO** Assignment 22: *Fork and strace*

## **TODO** Assignment 23: *Using gdb*

## **TODO** Assignment 24: *File permissions*

## **TODO** Assignment 25: *Heap vs. stack (difficult)*