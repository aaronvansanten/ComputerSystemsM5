# System Calls

## Assignment 1: *Echo*
The output of the `strace` looks as follows:
```
execve("./Echo", ["./Echo", "Hello", "World"], 0x7ffffff520 /* 24 vars */) = 0
brk(NULL)                               = 0x5555562000
faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=72939, ...}) = 0
mmap(NULL, 72939, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7ff7fba000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0`\17\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1455120, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7ff7ff8000
mmap(NULL, 1527752, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7ff7e45000
mprotect(0x7ff7fa2000, 61440, PROT_NONE) = 0
mmap(0x7ff7fb1000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15c000) = 0x7ff7fb1000
mmap(0x7ff7fb7000, 12232, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7ff7fb7000
close(3)                                = 0
mprotect(0x7ff7fb1000, 16384, PROT_READ) = 0
mprotect(0x5555560000, 4096, PROT_READ) = 0
mprotect(0x7ff7ffd000, 4096, PROT_READ) = 0
munmap(0x7ff7fba000, 72939)             = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
brk(NULL)                               = 0x5555562000
brk(0x5555583000)                       = 0x5555583000
write(1, "./Echo Hello World (3)\n", 23./Echo Hello World (3)) = 23
exit_group(0)                           = ?
+++ exited with 0 +++
```

Almost all calls are for the program initialization. However, the specific body calls are the following:
```
brk(NULL)                               = 0x5555562000
brk(0x5555583000)                       = 0x5555583000
write(1, "./Echo Hello World (3)\n", 23./Echo Hello World (3)) = 23
```
This call origins from the body and writes the input back to the terminal using the `write()`. This `write()` is a library call back to the included `<stdio.h>` library. Therefore, this system call originates from a library call. In this case, the libary calls seems to map 1:1 to system calls. However, this does not prove that library calls *always* map 1:1 on system calls, as explained [here](https://unix.stackexchange.com/questions/615872/can-a-library-call-invoke-more-than-one-system-call).

# Scheduling

## Assignment 2: *Loop*
The initial `N` value was 750. Running this took about 5 seconds. Therefore, I tried doubling it, but this resulted in a way higher time than aimed at. By slowly decrementing the value, I eventually ended at `N=1350`. This took 0m10.198s.

Based on the `loop.c` source code, N defines the time it takes to complete two loops. The higher N is, the more loop iterations the program needs to complete. Two loops use the variable. Therefore, an increase by 1 on `N` increases the number of iterations rather considerably. Based on this, I assume that `N` defines the number of CPU cycles that need to be completed (every loop iteration).


## Assignment 3: *SchedXY*
Comparing the times, the FIFO processes ran for about 11 seconds. The RobinRound processes ran for about 15 seconds.

Looking at the source code, I noticed that 2000 loop iterations were required before completion of the program. It seems that the FIFO structure completes these faster then the RobinRound process.


## Assignment 4: *Nice*
Running `./Nice >junk&` resulted in 9 processes created that were visible using `htop`. As can be seen from the file, the processes are finished in their given priorities. 

Running `./Nice x >junk&` resulted in the same results that was visible in `htop`. However, as can be seen from the file, the processes were now shuffled a bit instead of executing based on their priorities.


# I/O

## Assignment 5: *wiringpi*
I have upgraded the library using the terminal commands, since this was easier for me. 

The output of the `gpio readall` command printed the properties of all GPIO pins on the raspberry pi. The following pins were described:
| Pin | Function |
| - | - |
| 0v | Ground |
| 3.3v| An output pin giving 3.3 volts|
| 5v| An output pin giving 5 volts|
| GPIO| Pins used for input/output |
| TxD| UART transmit|
| RxD| UART receive|
| SDA| serial data, used in the I2C protocol  |
| SDC| serial clock, used in the I2C protocol  |
| MOSI| SPI protocol, master out - slave in|
| Miso| SPI protocol, master in - slave out|

## Assignment 6: *GPIO*
I created the following code:
``` bash
#!/bin/bash

gpio mode 0 output;

for i in {1..5}
do
  echo "I work";
  gpio toggle 0;
  sleep 1;
done
```
The pin number is 0 since this is the wiring ping according to the documentation. It blinks 5 times with a 1 second interval.

**INSERT IMAGE**

## Assignment 7: *Light dependent resistor*



## Assignment 8: *Blink*
Looking at the system calls, the program calls the kernel in the following way: `clock_nanosleep(CLOCK_REALTIME, 0, {tv_sec=0, tv_nsec=500000000}, 0x7ffffff380) = 0` when it is time to blink the LED. the `CLOCK_REALTIME` flag causes me to believe that the seconds are based on the real system time, and not the internal time of the CPU (such as wait a 1000 clock cycles). Therefore, when the system is clogged, it will not delay the blinking of the LED.


## Assignment 9: *Sparse*



## Assignment 10: *Fadvise*



## Assignment 11: *ln.sh*



## Assignment 12: *Pipe.c*



## Assignment 13: *Fifo.c*



## Assignment 14: *Readdir.c*



## Assignment 15: *fdisk*



## Assignment 16: *Filesystems on the pi*



## Assignment 18: *Mount.c*



## Assignment 18: *Unplug*



## Assignment 19: *Fadvise*



## Assignment 20: *Ext4*



## Assignment 21: *Ext4 read and write*



## Assignment 22: *Backup ext4*



## Assignment 23: *Unsafe removal*



## Assignment 24: *tail -f*



# Kernel

## Assignment 25: *lsmod*
