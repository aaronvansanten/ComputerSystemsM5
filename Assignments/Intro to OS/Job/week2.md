-# Fork, Signal and IPC

## Assignment 1: *Understanding Processes*

### a)
```
main()
+--+ fork1
|  +--+ fork11
|  |  +--- fork111
|  +--- fork12
|
+--+ fork2
|  +-- fork21
|
+--- fork3
```

### b)

```
main()
+--- fork1
|
+--- fork2
```

Fork 1 does not fork again, because `fork()` returns 0 to the child process. Therefore, the if statement returns false and no fork is done in the child process. However, in the main process the `fork()` returns the PID which is more than 0, therefore the if statement returns true and another fork is made.

### c)

```
main()
+-- fork1
+-- fork2
+-- fork3
```

While the counter is below three it will create a fork. However, the resulting child processes go through a similar process as in assignment 1b, they continue with the program at the if statement, but return 0 and therefore do not fork again.

## Assignment 2: *Working with Signals*

The program attempts to write to a reserved address (0x0). It is alright to point to 0x0, but not to write to 0x0.

## Assignment 3: *Modify Signal.c*

```C
int main(int argc, char * argv[]) {
    int tmp;
    int *pointer = &tmp;
    signal(SIGSEGV,signal_handler);
    printf("About to segfault:\n");
    *pointer=0;
    printf("Why didn't we crash?\n");
    return 1;
}
```

Now `pointer` points to a newly generated address instead 0x0.

## Assignment 4: *SIGSEGV*

I have decided to catch the `ctrl+c` keybind and output a message when that happens. To give time to press `ctrl+c`, I added a sleep(5).

The modified code:

```C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>

void signal_handler(int sig) {
    printf("Signal %d received.\n", sig);
    exit(0);
}

void cancel_handler(int sig) {
    printf("\nWhy you kill me?\n");
    exit(0);
}

int main(int argc, char * argv[]) {
    int tmp;
    int *pointer = &tmp;
    signal(SIGSEGV,signal_handler);
    signal(SIGINT,cancel_handler);
    printf("About to segfault:\n");
    *pointer=0;
    printf("Why didn't we crash?\n");
    sleep(5);
    return 1;
}
```

## Assignment 5: *Trying it all Together*

I added a `fork()` after `int *pointer = &tmp`. The output after pressing `ctrl+c` is duplicated:

```
About to segfault:
Why didn't we crash?
About to segfault:
Why didn't we crash?
^C

Why you kill me?
Why you kill me?
```

Therefore the child process has handled the signal.

# Threads

## Assignment 6: *Basics of Threads*

`DashFunctions` executes two functions (`dash()` and `underscore`) one after the other. `DashThreads` creates two processes which simultaneously output to the terminal. This explains why `DashFunctions` has the characters nicely sorted, while `DashThreads` has them in a somewhat random order as the threads execute at the same time.

## Assignment 7: *Working with Threads*

By increasing `N` to 50000 I have found that the RPi can create 9086 threads. My laptop stopped at 18773 threads while a number of other programs were running.

The system call responsible is:

```C
clone(child_stack=0x7f90f48ac0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTIDThread 9
, parent_tid=[20952], tls=0x7f90f498c0, child_tidptr=0x7f90f49290) = 20952
```

## Assignment 8: *MyThread Timing*

Both programs were set to create 1500 threads to amplify the measurements.

| Time | C      | Java   |
|------|--------|--------|
| real | 0.150s | 1.208s |
| user | 0.028s | 1.564s |
| sys  | 0.216s | 0.817s |

As can be seen, the C version is significantly faster than the Java version.

I believe this is the case because Java creates a sort of virtual machine to provide the cross-platform compatability that Java offers. However, this does significantly increase the number of instructions the program has to execute. This can be seen when running `strace -f java MyThread` and looking at the shear number of system calls. The C version just doesn't have this complexity.


## Assignment 9: *Power of Threads*

I wrote the following program:

```C
#include <stdio.h>
#include <pthread.h>

double result = 0.0;
double v[] = {1,2,3,4,5};
double u[] = {6,7,8,9,0};

void adder(int n) {
    result += v[n] * u[n];
    // printf("The result so far is: %f\n", result);
}

void scalar_product_seq (int n) {
    pthread_t tid;
    for ( int i = 0; i < n ; i++) {
        pthread_create(&tid, NULL, adder, i); // don't use 'n' here as you did before...
    }
    for ( int i = 0; i < n; i++) {
        pthread_join(tid, NULL);
    }
    printf("%f\n", result);
    pthread_exit(NULL);
}

int main(int argc, char * argv[]) {
    // use ./poft <n> to specify how far to go. No error handling!
    int n_argument = strtol(argv[1], NULL, 10); 
    scalar_product_seq(n_argument);
}
```

The program can be called using `./poft <n>`. However, the arrays are hard coded. The program creates a new thread for every multiplication that needs to happen and adds up the result.

# Concurrency

## Assignment 10: *Parallel shell*

The command consists of several parts:

`cat index.html`: prints the contents of `index.html` to the pipe.

`sort`: sorts the output.

`uniq -c`: only displays unique lines once and prints how often they occur.

`sort -rn`: sorts the output in reverse based on numeric input. This effectively sorts by most occuring lines.

`pr -2`: prints the output in two columns.

In this case `sort` is redundant, but if it needs to happen this could be on a separate thread, therefore allowing for concurrency. However, the `uniq` command does not require the input to be sorted. Subsequent commands do rely on the `uniq` command output and therefore cannot be processed concurrently.

## Assignment 11: *Count.java*

Sample output:

```
pi@pi-logmans:~/week2/files $ java -ea Count
inc	ctr	SEM=false

-1	-1
1	1
1	2
1	3
1	4
1	5
-1	-2
-1	-3
-1	-4
-1	-5
-1	-6
-1	-7
-1	-8
-1	-9
-1	-10
1	6
1	7
1	8
1	9
1	10
Final value ctr=10
Exception in thread "main" java.lang.AssertionError
	at Count.main(Count.java:40)
```

The assertion error was consistent. However, the counting was seemingly random. The final value would always be `-10` or `10`. This is most probably because the threads that are counting are racing each other and do not consistently execute in the same order.

## Assignment 12: *Count with Semaphore*

Executing with `java -ea Count x` does alter the results. It consistently counts up and back down again (or in the reversed order) and always has final result `0`. However, it is not consistent in whether it counts up or down first. It also does not show an assertion error anymore.

Concurrency is not very useful anymore as the results are clearly sequential.

## Assignment 13: *Count in C and Java*

For the C version I used `./Count -ea`. Both programs have the same end result, but C has much more switching in counting up or down, implying that the semaphore is released after every increment/decrement. Java does not have this as one process holds on to the semaphore until it is finished.

This does result in significantly faster processing times:

| Time | C      | Java   |
|------|--------|--------|
| real | 0.009s | 0.470s |
| user | 0.001s | 0.597s |
| sys  | 0.008s | 0.109s |

## Assignment 14: *Count and strace*

The command has three outputs:

```
pi@pi-logmans:~/week2/files $ cat foo.1522
execve("./Count", ["./Count"], 0x7ff8e76018 /* 32 vars */) = 0
brk(NULL)                               = 0x557f225000
faccessat(AT_FDCWD, "/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=72939, ...}) = 0
mmap(NULL, 72939, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fac930000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0 a\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=160200, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fac96e000
mmap(NULL, 197632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fac8ff000
mprotect(0x7fac91b000, 61440, PROT_NONE) = 0
mmap(0x7fac92a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b000) = 0x7fac92a000
mmap(0x7fac92c000, 13312, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fac92c000
close(3)                                = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0`\17\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1455120, ...}) = 0
mmap(NULL, 1527752, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fac78a000
mprotect(0x7fac8e7000, 61440, PROT_NONE) = 0
mmap(0x7fac8f6000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x15c000) = 0x7fac8f6000
mmap(0x7fac8fc000, 12232, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fac8fc000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fac96c000
mprotect(0x7fac8f6000, 16384, PROT_READ) = 0
mprotect(0x7fac92a000, 4096, PROT_READ) = 0
mprotect(0x55642f1000, 4096, PROT_READ) = 0
mprotect(0x7fac973000, 4096, PROT_READ) = 0
munmap(0x7fac930000, 72939)             = 0
set_tid_address(0x7fac96c110)           = 1522
set_robust_list(0x7fac96c120, 24)       = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7fac904b94, sa_mask=[], sa_flags=SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7fac904c50, sa_mask=[], sa_flags=SA_RESTART|SA_SIGINFO}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
brk(NULL)                               = 0x557f225000
brk(0x557f246000)                       = 0x557f246000
write(1, "inc\tctr\tSwitch=false\n", 21) = 21
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fabf89000
mprotect(0x7fabf8a000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fac788ac0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[1523], tls=0x7fac7898c0, child_tidptr=0x7fac789290) = 1523
mmap(NULL, 8392704, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7fab788000
mprotect(0x7fab789000, 8388608, PROT_READ|PROT_WRITE) = 0
clone(child_stack=0x7fabf87ac0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tid=[1524], tls=0x7fabf888c0, child_tidptr=0x7fabf88290) = 1524
futex(0x7fac789290, FUTEX_WAIT, 1523, NULL) = 0
write(2, "Count: Count.c:46: main: Asserti"..., 54) = 54
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fac96b000
rt_sigprocmask(SIG_UNBLOCK, [ABRT], NULL, 8) = 0
rt_sigprocmask(SIG_BLOCK, ~[RTMIN RT_1], [], 8) = 0
getpid()                                = 1522
gettid()                                = 1522
tgkill(1522, 1522, SIGABRT)             = 0
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
--- SIGABRT {si_signo=SIGABRT, si_code=SI_TKILL, si_pid=1522, si_uid=1000} ---
+++ killed by SIGABRT +++
```

And:

```
pi@pi-logmans:~/week2/files $ cat foo.1523
set_robust_list(0x7fac7892a0, 24)       = 0
write(1, "1\t1\n", 4)                   = 4
sched_yield()                           = 0
write(1, "1\t2\n", 4)                   = 4
sched_yield()                           = 0
write(1, "1\t3\n", 4)                   = 4
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 1
sched_yield()                           = 0
write(1, "1\t4\n", 4)                   = 4
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "1\t5\n", 4)                   = 4
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = 0
write(1, "1\t6\n", 4)                   = 4
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "1\t7\n", 4)                   = 4
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = 0
write(1, "1\t8\n", 4)                   = 4
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "1\t9\n", 4)                   = 4
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = 0
write(1, "1\t10\n", 5)                  = 5
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=72939, ...}) = 0
mmap(NULL, 72939, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fac930000
close(3)                                = 0
mmap(NULL, 134217728, PROT_NONE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_NORESERVE, -1, 0) = 0x7fa3788000
munmap(0x7fa3788000, 8880128)           = 0
munmap(0x7fa8000000, 58228736)          = 0
mprotect(0x7fa4000000, 135168, PROT_READ|PROT_WRITE) = 0
openat(AT_FDCWD, "/lib/aarch64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0\267\0\1\0\0\0\320)\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=80200, ...}) = 0
mmap(NULL, 144472, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fab764000
mprotect(0x7fab777000, 61440, PROT_NONE) = 0
mmap(0x7fab786000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x12000) = 0x7fab786000
close(3)                                = 0
mprotect(0x7fab786000, 4096, PROT_READ) = 0
munmap(0x7fac930000, 72939)             = 0
futex(0x7fac9749e8, FUTEX_WAKE_PRIVATE, 1) = 1
futex(0x7fab787234, FUTEX_WAKE_PRIVATE, 2147483647) = 0
madvise(0x7fabf89000, 8253440, MADV_DONTNEED) = 0
exit(0)                                 = ?
+++ exited with 0 +++
```

And:

```
pi@pi-logmans:~/week2/files $ cat foo.1524
set_robust_list(0x7fabf882a0, 24)       = 0
write(1, "-1\t0\n", 5)                  = 5
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = 0
write(1, "-1\t-1\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = -1 EAGAIN (Resource temporarily unavailable)
write(1, "-1\t-2\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "-1\t-3\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 1
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = -1 EAGAIN (Resource temporarily unavailable)
write(1, "-1\t-4\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "-1\t-5\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 1
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = -1 EAGAIN (Resource temporarily unavailable)
write(1, "-1\t-6\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "-1\t-7\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 1
sched_yield()                           = 0
futex(0x7fac8fc750, FUTEX_WAIT_PRIVATE, 2, NULL) = -1 EAGAIN (Resource temporarily unavailable)
write(1, "-1\t-8\n", 6)                 = 6
futex(0x7fac8fc750, FUTEX_WAKE_PRIVATE, 1) = 0
sched_yield()                           = 0
write(1, "-1\t-9\n", 6)                 = 6
sched_yield()                           = 0
futex(0x7fac9749e8, FUTEX_WAIT_PRIVATE, 2, NULL) = 0
futex(0x7fac9749e8, FUTEX_WAKE_PRIVATE, 1) = 0
madvise(0x7fab788000, 8253440, MADV_DONTNEED) = 0
exit(0)                                 = ?
+++ exited with 0 +++
```

The main system call responsible for the semaphore is `futex()`. This system call can check whether the resource (in this case the variable) is available and will set it to `Resource temporarily unavailable` if the process can use it. Once succesful is will write the value to the resource. Otherwise it will yield and wait to write.

## Assignment 15: *Spinlock*

`Spinlock` defines a lock on a variable and until the lock is set to `0` the `for` loop keeps counting up `i`. With the `-dyield` option set, the program will yield the CPU for a cycle, freeing up resources. By doing that it also counts less often as can be seen in the results. Without `dyield` it counts to somewhere in the range of `98512046`, and with `-dyield` it only reaches `320866`.

## Assignment 16: *Spinlock with processes (difficult)*



## Assignment 18: *ProdCons*

The output is as follows:

```
pi@pi-logmans:~/week2/files $ ./ProdCons1
    B[0]=0
i=0 0=B[0]
    B[0]=1
i=1 1=B[0]
    B[0]=2
i=2 2=B[0]
    B[0]=3
i=3 3=B[0]
    B[0]=4
i=4 4=B[0]
    B[0]=5
i=5 5=B[0]
    B[0]=6
i=6 6=B[0]
    B[0]=7
i=7 7=B[0]
    B[0]=8
i=8 8=B[0]
    B[0]=9
i=9 9=B[0]
```

And:

```
pi@pi-logmans:~/week2/files $ ./ProdCons4
    B[0]=0
    B[1]=1
    B[2]=2
    B[3]=3
i=0 0=B[0]
i=1 1=B[1]
i=2 2=B[2]
i=3 3=B[3]
    B[0]=4
    B[1]=5
    B[2]=6
    B[3]=7
i=4 4=B[0]
i=5 5=B[1]
i=6 6=B[2]
i=7 7=B[3]
    B[0]=8
    B[1]=9
i=8 8=B[0]
i=9 9=B[1]
```

With a lower `N` the threads are more synchronized.

## Assignment 19: *ProdCons initialisation*

Sample output (it is quite random):

```
pi@pi-logmans:~/week2/files $ ./ProdConsPlus1
    B[0]=0
    B[1]=1
    B[2]=2
    B[3]=3
    B[0]=4
i=0 0=B[0]
i=1 1=B[1]
    B[1]=5
    B[2]=6
i=2 2=B[2]
i=3 3=B[3]
i=4 4=B[0]
i=5 5=B[1]
    B[3]=7
    B[0]=8
    B[1]=9
i=6 6=B[2]
i=7 7=B[3]
i=8 8=B[0]
i=9 9=B[1]
```

However, it does quite often crash with:

```
ProdConsPlus1: ProdCons.c:36: tcons: Assertion `i==k' failed.
Aborted
```

By initializing `Spaces` with `N+1` there is too much room in the buffer. This can result in the `tcons` thread to fail its assertion that `i==k. This can also be observed in one of the crashes:

```
pi@pi-logmans:~/week2/files $ ./ProdConsPlus1 
    B[0]=0
    B[1]=1
    B[2]=2
    B[3]=3
    B[0]=4
i=0 4=B[0]
ProdConsPlus1: ProdCons.c:36: tcons: Assertion `i==k' failed.
Aborted
```

Where it can clearly be seen that `i` is not `k`.

## Assignment 20: *ProdManyCons*

Without the `-x` the program fails most of the time with:

```
ProdManyCons: ProdManyCons.c:32: tcons: Assertion `sum == (P*M-1)*P*M/2' failed.
Aborted
```

With this program there are many consumers who all try to access the buffer at the same time. By passing `-x` to the program it enables semaphores and thereby protects the resource from corruption.

## Assignment 21: *ProdCons sem_wait Spaces*



## Assignment 22: *ProdCons sem_wait Elements*


# Memory

## Assignment 23: *ProcessLayout*


# Virtual Memory

## Assignment 24: *Getrusage*



## Assignment 25: *Getrusage direct*

## Assignment 26: *Mmap*


## Assignment 27: *Mmap with argument*
