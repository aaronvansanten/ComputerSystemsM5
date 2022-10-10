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

## Assignment 15: *Get familiar with C*

Apart from a small difference in which libraries are called, Vname.c uses the arrow operator to reference the desired variables instead of the dot operator. The dot operator is normally used to reference variables of a structure, while the arrow operator accesses the variables which are stored with pointers instead  of references. This is also reflected in how Vname.c allocates memory for the utsname structure and Uname.c creates the utsname structure using referencing.

Both programs output `pi-logmans Linux 5.15.61-v8+ aarch64`.

## Assignment 16: *Syscalls*

The first difference noticed between my laptop and the Pi is that my laptop has a number of `arch_prctl` calls which are specific to the x86-64 architecture. Therefore the Pi (which runs on ARM) should not have these calls. On my laptop the arch_prctl is followed by a memory map command `mmap` after which the `access` call is made. Then the Pi and laptop make a very similar call with `access` and `faccessat` (they differ mainly in how the filepath is treated, but have the same purpose). There are several other differences, but the biggest one is that my laptop uses a lot more `pread64` calls, where the Pi uses none. It seems that my laptop has much more memory operations and more support/implementation for multithreaded applications as can be seen from the manual pages of these calls.

Subsequent execution of the program does not differ in output apart from the memory addresses which is to be expected as the OS assigns different virtual memory addresses everytime.

## Assignment 17: *Monitor processes on the Pi*

I chose to use htop as this is what I'm used to on my main system and it is already installed on the Pi. It automatically sorts the processes based on their CPU usage which answers the first question.

To view the disk I/O I had to add this column to the view which could be done in setup -> Columns -> Available Columns -> IO_RATE.

## Assignment 18: *Address space*

The first memory address is the address of the program itself. The second is the address of the program text. Third is a variable defined outside the main function. Fourth is edata, which is initialized data. Then there is a variable defined within the main function. Then `end` is unitialized data. 'd' is the memory address of a pointer. `brk` is the last address the program can currently use for its data. Then more memory is allocated in the lines after that. The new address of `d` is printed and the new memory bound is printed.

`malloc` is used to allocate memory for a program where the program can store its data. This then needs to be freed using `free` after it is no longer used. Otherwise this memory address will only be available once the program exits, which could lead to memory leaks and instability if not controlled. `alloca` allocates memory but automatically frees it once the function that called `alloca()` returns to its caller.

## Assignment 19: *Stack layout*

`./StackLayout` has the following output:

```
Stack:
0x7fd43d659c:argc
0x7fd43d6590:argv
0x7fd43d6588:envp
0x7fd43d65ac:a
0x7fd43d65a8:b
0x7fd43d6568:c
0x7fd43d6560:d
0x7fd43d653c:e
0x7fd43d6538:f
0x7fd43d654c:g
0x7fd43d6548:h
0x7fd43d6540:p
```

This program has three functions, `main`, `foo`, and `bar` with each function adding variables to the stack. `argc`, `argv`, and `envp` are variables belonging to `main`, and `a` and `b` are also created there in the code.

From `main`, `foo` is called with parameters `c` and `d` which is added to the stack. `foo` then calls `bar` with `e` and `f`. `bar` also adds `g` and `h` to the stack. Lastly, `bar` also creates the variable `p` which is also added on the stack. However, minding the order of the memory order of the stack it should rather be printed as:

```
0x7fd43d65ac:a
0x7fd43d65a8:b
0x7fd43d659c:argc
0x7fd43d6590:argv
0x7fd43d6588:envp
0x7fd43d6568:c
0x7fd43d6560:d
0x7fd43d654c:g
0x7fd43d6548:h
0x7fd43d6540:p
0x7fd43d653c:e
0x7fd43d6538:f
```

Which also makes sense once one looks at the code as this is the order in which the variables are used. `argc`, `argv`, and `envp` are never defined or called until after `a` and `b` are initialized and therefore don't appear on the stack until then.

## Assignment 20: *BenchMem*

| Allocated memory | System Time | Comments |
|------------------|-------------|----------|
| 1024             | 1.357       |          |
| 2048             | 2.576       |          |
| 3072             | 4.301       |          |
| 3800             | 4.658       |          |
| 3900             | 4.449       |          |
| 3900             | 4.844       | after reboot |

It seems that the time necessary to allocate a certain size of memory increases linearly. However, it was noticed that while the `sys` time (using the `time` command) increased linearly, the `real` time did seem to grow faster, but this was very inconsistent.

## Assignment 21: *Creating Processes using Fork*

`strace` does not trace the child process by default. `clone()` is the system call for creating the child process. By using `fork -f ./Fork` one can trace the system calls of the child process as well under its new pid.

## Assignment 22: *Fork and strace*

I could not make the child process appear in `htop`. I have tried adding a `sleep(5);` to the Fork program, sorting and filtering within `htop` for the correct pid, spamming the program and more.

By adding a `sleep(5)` to the code I was able to make the thread appear in `htop`. I also added a filter in `htop` for 'Fork'. See the following screenshot:

![](../../../.images/PID%20Job.png)

## Assignment 23: *Using gdb*

The provided commands yield the following output:

```
Reading symbols from ./numberOfArguments...
(gdb) disassemble add
Dump of assembler code for function add:
   0x0000000000000804 <+0>:	sub	sp, sp, #0x10
   0x0000000000000808 <+4>:	str	x0, [sp, #8]
   0x000000000000080c <+8>:	str	w1, [sp, #4]
   0x0000000000000810 <+12>:	ldr	x0, [sp, #8]
   0x0000000000000814 <+16>:	ldr	w1, [x0]
   0x0000000000000818 <+20>:	ldr	w0, [sp, #4]
   0x000000000000081c <+24>:	add	w0, w1, w0
   0x0000000000000820 <+28>:	add	sp, sp, #0x10
   0x0000000000000824 <+32>:	ret
End of assembler dump.
```

`sub sp, sp, #0x10`: subtracts decimal 16 from the sp register.

`str x0, [sp, #8]`: stores sp with byte offset 8 in x0.

`str w1, [sp, #4]`: stores sp with byte offset 4 in w1.

`ldr x0, [sp, #8]`: loads sp with byte offset 8 to x0.

`ldr w1, [x0]`: loads x0 to w1.

`ldr w0, [sp, #4]`: loads sp with byte offset 4 to w0.

`add w0, w1, w0`: adds w1 and w0 into w0.

`add sp, sp, #0x10`: adds decimal 16 to sp.

`ret`: returns from the subroutine.

## Assignment 24: *File permissions*

By using `ls -l | grep <filename>` I can easily check the permissions. `/etc/passwd` can only be written by root, other users can read. `/etc/shadow` can be written and read by root, but cannot even be viewed by non-root users.

For `~/.bashrc` I needed to add the `-a` tag to `ls` to view hidden files. This revealed that only the owner of the file (`pi` in my case) can write to this file, other users can only read.

To conclude, `/etc/passwd` and `/etc/shadow` require the use of `sudo`, while `~/.bashrc` does not.

## Assignment 25: *Heap vs. stack (difficult)*

The heap requires more instructions to store and retrieve data. This conceptually makes sense as the process needs to take more into account. The stack simply requires the location of the top, and nothing more. However, once a variable needs to be retrieved from a place that is not the top of the stack this could prove more difficult. How much slower this is depends on the program and in what way it uses the heap and/or stack. If memory access is limited and does not require access to items deep in the stack, I believe the stack to be faster. In situations with lots of memory access in deep locations, I believe the heap to be faster.