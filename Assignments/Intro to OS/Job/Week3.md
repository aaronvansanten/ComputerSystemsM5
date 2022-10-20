# System Calls

## Assignment 1: *Echo*

The output is as follows:

```
pi@pi-logmans:~/week2/files $ strace ./Echo Hello World
execve("./Echo", ["./Echo", "Hello", "World"], 0x7fcb909350 /* 32 vars */) = 0
brk(NULL)                               = 0x557365d000
faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=72939, ...}) = 0
mmap(NULL, 72939, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7faafb1000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0`\17\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1455120, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7faafef000
mmap(NULL, 1527752, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7faae3c000
mprotect(0x7faaf99000, 61440, PROT_NONE) = 0
mmap(0x7faafa8000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15c000) = 0x7faafa8000
mmap(0x7faafae000, 12232, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7faafae000
close(3)                                = 0
mprotect(0x7faafa8000, 16384, PROT_READ) = 0
mprotect(0x55587e0000, 4096, PROT_READ) = 0
mprotect(0x7faaff4000, 4096, PROT_READ) = 0
munmap(0x7faafb1000, 72939)             = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
brk(NULL)                               = 0x557365d000
brk(0x557367e000)                       = 0x557367e000
write(1, "./Echo Hello World (3)\n", 23./Echo Hello World (3)
) = 23
exit_group(0)                           = ?
+++ exited with 0 +++
```

Where only the following calls are specific to the program:

```
brk(NULL)                               = 0x557365d000
brk(0x557367e000)                       = 0x557367e000
write(1, "./Echo Hello World (3)\n", 23./Echo Hello World (3)
) = 23
```

All other commands are for the program initialization.

There is one function from a library: `printf()` from `stdio.h`. This is translated to a syscall `write()` from the `util-linux` package. From this it can be deducted that library calls are compiled to system calls, but system calls can also be in system libraries.

# Scheduling

## Assignment 2: *Loop*

By changing `N` to `1400` it takes 10.9 seconds to execute on my Pi.

## Assignment 3: *SchedXY*

As I could not observe any difference in scheduling I started a lot more processes: `sudo ./RR80& sudo ./RR80& sudo ./RR80& sudo ./RR80& sudo ./FIFO80& sudo ./FIFO80&`. From this it could be observed that the FIFO processes finished earlier than the Round-Robin processes, even though they were started later. While the observation was quite hard because `htop` had trouble transmitting over SSH with this load, I believe that the Round-Robin processes yielded to the FIFO processes and these then hogged the CPU until they were done.

## Assignment 4: *Nice*

`Nice.c` creates a lot of processes and assigns them different priorities. From `htop` and the output file it can be seen that the processes finish in their order of priority. However, all processes are executed on the same processor. With the extra argument it shuffles the priorities around.

# I/O

## Assignment 5: *wiringpi*



## Assignment 6: *GPIO*



## Assignment 7: *Light dependent resistor*



## Assignment 8: *Blink*



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
