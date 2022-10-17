# Fork, Signal and IPC

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

`Spinlock` defines a lock on a variable and until the lock is set to `0` the `for` loop keeps counting up `i`. With the `-dyield` option set, the program will yield the CPU for a cycle, freeing up resources. By doing that it also counts less often as can be seen in the results. Without `dyield` it counts to somewhere in the range of `98512046`, and with `-dyield` it only reaches `320866`. This is basically a kind of semaphore.

## Assignment 16: *Spinlock with processes (difficult)*

Unfortunately I did not manage to achieve this. The code I had is as follows:

```C
/*  Spinlock.c - Demonstrate the compare-and-swap instruction by releasing
    a lock after 1 second. */

#include <stdio.h>
#include <unistd.h>
#include <pthread.h>
#include <stdlib.h>

int Lock = 1;

void *tproc(void *ptr) {
    sleep(1);
    Lock = 0;
    printf("thread lock addr.: %p\n",&Lock);
    exit(0);
}

int main(int argc, char * argv[]) {
    int i;
    if (fork() == 0) { // if this is child process:
        tproc(NULL);
    } else { // if still in the parent process:
        printf("1 Lock addr.: %p\n",&Lock);
        for(i=0; !__sync_bool_compare_and_swap(&Lock, 0, 2); i++) {
            sched_yield();
        }
        printf("Lock addr.: %p\n",&Lock);
        printf("Waited loops: %d\n",i);
        return 0;
    }
}

/*  Please note that the checks on the return value of the system calls
    have been omitted to avoid cluttering the code. However, system calls
    can and will fail, in which case the results are unpredictable. */
```

I believe the solution to be the creation of shared memory in which the lock resides. However, I did not have time to implement this as this is not a small task.

## Assignment 17: *ProdCons*

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

## Assignment 18: *ProdCons initialisation*

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

## Assignment 19: *ProdManyCons*

Without the `-x` the program fails most of the time with:

```
ProdManyCons: ProdManyCons.c:32: tcons: Assertion `sum == (P*M-1)*P*M/2' failed.
Aborted
```

With this program there are many consumers who all try to access the buffer at the same time. By passing `-x` to the program it enables semaphores and thereby protects the resource from corruption.

## Assignment 20: *ProdCons sem_wait Spaces*

After commenting out the specified line we get the following output:

```
pi@pi-logmans:~/week2/files $ ./ProdCons.exe 
    B[0]=0
    B[1]=1
    B[2]=2
    B[3]=3
    B[0]=4
    B[1]=5
    B[2]=6
    B[3]=7
    B[0]=8
    B[1]=9
i=0 0=B[0]
i=1 9=B[1]
ProdCons.exe: ProdCons.c:36: tcons: Assertion `i==k' failed.
Aborted
```

It is not very consistent, but the idea is generally the same.

Because the `sem_wait()` is removed the semaphore is no longer locked and the consumer and producer simultaneously work with the buffer and `i==k` fails.

## Assignment 21: *ProdCons sem_wait Elements*

With the specified modification the program fails earlier. They are usually along the lines of:

```
pi@pi-logmans:~/week2/files $ ./ProdCons.exe 
    B[0]=0
i=0 0=B[0]
i=1 0=B[1]
    B[1]=1
    B[2]=2
ProdCons.exe: ProdCons.c:36: tcons: Assertion `i==k' failed.
Aborted
```

With this modification the consumer is never stopped and therefore consumes at the same time as the producer produces. Therefore the assertion `i==k` fails.

# Memory

## Assignment 22: *ProcessLayout*

```
pi@pi-logmans:~/week2/files $ gcc ProcessLayout.c -o ProcessLayout -lpthread
pi@pi-logmans:~/week2/files $ ./ProcessLayout 
pid=1534, tid=0x7fb8899040
sbrk 0x559e2e9000
tid=0x7fb86b61c0 stack=0x7fb86b5998
sbrk 0x559e3e9000
tid=0x7fb7eb51c0 stack=0x7fb7eb4998
sbrk 0x559e4e9000
tid=0x7fb76b41c0 stack=0x7fb76b3998
1534:   ./ProcessLayout
Address           Kbytes     RSS   Dirty Mode  Mapping
0000005588a90000       4       4       4 r-x-- ProcessLayout
0000005588aa1000       4       4       4 r---- ProcessLayout
0000005588aa2000       4       4       4 rw--- ProcessLayout
000000559e2c8000    3204       4       4 rw---   [ anon ]
0000007fb6eb4000       4       0       0 -----   [ anon ]
0000007fb6eb5000    8192       8       8 rw---   [ anon ]
0000007fb76b5000       4       0       0 -----   [ anon ]
0000007fb76b6000    8192       8       8 rw---   [ anon ]
0000007fb7eb6000       4       0       0 -----   [ anon ]
0000007fb7eb7000    8192       8       8 rw---   [ anon ]
0000007fb86b7000    1396     932       0 r-x-- libc-2.31.so
0000007fb8814000      60       0       0 ----- libc-2.31.so
0000007fb8823000      16      16      16 r---- libc-2.31.so
0000007fb8827000       8       8       8 rw--- libc-2.31.so
0000007fb8829000      12      12      12 rw---   [ anon ]
0000007fb882c000     112     112       0 r-x-- libpthread-2.31.so
0000007fb8848000      60       0       0 ----- libpthread-2.31.so
0000007fb8857000       4       4       4 r---- libpthread-2.31.so
0000007fb8858000       4       4       4 rw--- libpthread-2.31.so
0000007fb8859000      16       4       4 rw---   [ anon ]
0000007fb886f000     136     132       0 r-x-- ld-2.31.so
0000007fb8899000      16       8       8 rw---   [ anon ]
0000007fb889d000       8       0       0 r----   [ anon ]
0000007fb889f000       4       4       0 r-x--   [ anon ]
0000007fb88a0000       4       4       4 r---- ld-2.31.so
0000007fb88a1000       8       8       8 rw--- ld-2.31.so
0000007ff9943000     132      12      12 rw---   [ stack ]
---------------- ------- ------- ------- 
total kB           29800    1300     120
```

Then with `N` changed to 2:

```
pi@pi-logmans:~/week2/files $ ./ProcessLayout 
pid=1556, tid=0x7fb282b040
sbrk 0x5578fde000
tid=0x7fb26481c0 stack=0x7fb2647998
sbrk 0x55790de000
tid=0x7fb1e471c0 stack=0x7fb1e46998
1556:   ./ProcessLayout
Address           Kbytes     RSS   Dirty Mode  Mapping
0000005576d60000       4       4       4 r-x-- ProcessLayout
0000005576d71000       4       4       4 r---- ProcessLayout
0000005576d72000       4       4       4 rw--- ProcessLayout
0000005578fbd000    2180       4       4 rw---   [ anon ]
0000007fb1647000       4       0       0 -----   [ anon ]
0000007fb1648000    8192       8       8 rw---   [ anon ]
0000007fb1e48000       4       0       0 -----   [ anon ]
0000007fb1e49000    8192       8       8 rw---   [ anon ]
0000007fb2649000    1396     924       0 r-x-- libc-2.31.so
0000007fb27a6000      60       0       0 ----- libc-2.31.so
0000007fb27b5000      16      16      16 r---- libc-2.31.so
0000007fb27b9000       8       8       8 rw--- libc-2.31.so
0000007fb27bb000      12      12      12 rw---   [ anon ]
0000007fb27be000     112     112       0 r-x-- libpthread-2.31.so
0000007fb27da000      60       0       0 ----- libpthread-2.31.so
0000007fb27e9000       4       4       4 r---- libpthread-2.31.so
0000007fb27ea000       4       4       4 rw--- libpthread-2.31.so
0000007fb27eb000      16       4       4 rw---   [ anon ]
0000007fb2801000     136     124       0 r-x-- ld-2.31.so
0000007fb282b000      16       8       8 rw---   [ anon ]
0000007fb282f000       8       0       0 r----   [ anon ]
0000007fb2831000       4       4       0 r-x--   [ anon ]
0000007fb2832000       4       4       4 r---- ld-2.31.so
0000007fb2833000       8       8       8 rw--- ld-2.31.so
0000007fe87ff000     132      12      12 rw---   [ stack ]
---------------- ------- ------- ------- 
total kB           20580    1276     112
```


With `N=2` only one thread is created and therefore two lines are missing as compared to `N=3`. It can be seen that most of the memory is shared. There are a couple differences, that being the following lines:

```
sbrk 0x559e4e9000
tid=0x7fb76b41c0 stack=0x7fb76b3998
0000007fb7eb6000       4       0       0 -----   [ anon ]
0000007fb7eb7000    8192       8       8 rw---   [ anon ]
```

# Virtual Memory

## Assignment 23: *Getrusage*

After compilation and running with and without `x` the output is as follows:

```
pi@pi-logmans:~/week2/files $ ./Getrusage 
cnt=       0, flt=107
cnt=      f8, flt=108
cnt=    10f8, flt=109
cnt=    20f8, flt=110
cnt=    30f8, flt=111
cnt=    40f8, flt=112
cnt=    50f8, flt=113
Lock=off

pi@pi-logmans:~/week2/files $ ./Getrusage x
cnt=       0, flt=112
Lock=on
```

With the lock enabled, the memory is blocked from being moved into the swap file/partition. This program measures the number of soft page faults/reclaims with the `usage.ru_minflt` variable. It will then print the number of times an address was accessed from swap.

With the lock enabled no swap is ever used as the memory is locked in RAM.

## Assignment 24: *Getrusage direct*

```
pi@pi-logmans:~/week2/files $ gcc Getrusage.c -o Getrusage -DDIRECT
pi@pi-logmans:~/week2/files $ ./Getrusage
cnt=     d98, flt=105
cnt=     d99, flt=113
cnt=    1d98, flt=114
cnt=    2d98, flt=115
cnt=    3d98, flt=116
cnt=    4d98, flt=117
Lock=off
pi@pi-logmans:~/week2/files $ nano Getrusage.c 
pi@pi-logmans:~/week2/files $ ./Getrusage
cnt=     3b8, flt=107
cnt=     3b9, flt=115
cnt=    13b8, flt=116
cnt=    23b8, flt=117
cnt=    33b8, flt=118
cnt=    43b8, flt=119
cnt=    53b8, flt=120
Lock=off
pi@pi-logmans:~/week2/files $ ./Getrusage
cnt=     238, flt=107
cnt=     239, flt=113
cnt=    1238, flt=114
cnt=    2238, flt=115
cnt=    3238, flt=116
cnt=    4238, flt=117
cnt=    5238, flt=118
Lock=off
```

I do not observe a difference. Could it be that the compiler already compiled with the `-DDIRECT` flag even though I did not specify it?

## Assignment 25: *Mmap*

The command generated the following output:

```
pi@pi-logmans:~/week2/files $ ./Mmap Mmap.c foo
1960:   ./Mmap Mmap.c foo
Address           Kbytes     RSS   Dirty Mode  Mapping
0000005588b70000       4       4       4 r-x-- Mmap
0000005588b81000       4       4       4 r---- Mmap
0000005588b82000       4       4       4 rw--- Mmap
0000007fa066f000    1396     964       0 r-x-- libc-2.31.so
0000007fa07cc000      60       0       0 ----- libc-2.31.so
0000007fa07db000      16      16      16 r---- libc-2.31.so
0000007fa07df000       8       8       8 rw--- libc-2.31.so
0000007fa07e1000      12      12      12 rw---   [ anon ]
0000007fa07f6000     136     136       0 r-x-- ld-2.31.so
0000007fa0820000       4       4       4 -w-s- foo
0000007fa0821000       4       4       0 r---- Mmap.c
0000007fa0822000       8       8       8 rw---   [ anon ]
0000007fa0824000       8       0       0 r----   [ anon ]
0000007fa0826000       4       4       0 r-x--   [ anon ]
0000007fa0827000       4       4       4 r---- ld-2.31.so
0000007fa0828000       8       8       8 rw--- ld-2.31.so
0000007fdeb7c000     132      12      12 rw---   [ stack ]
---------------- ------- ------- ------- 
total kB            1812    1192      84
```

First the file is loaded into memory and then copied using `memcpy`. This is done with the `MAP_SHARED` flag which makes all changes to the mapping visible to other processes. With `cat foo` the file (which is nothing more than a mapping to a memory address) can be output.

## Assignment 26: *Mmap with argument*

```
pi@pi-logmans:~/week2/files $ ./Mmap Mmap.c bar x
1500:   ./Mmap Mmap.c bar x
Address           Kbytes     RSS   Dirty Mode  Mapping
00000055596c0000       4       4       0 r-x-- Mmap
00000055596d1000       4       4       4 r---- Mmap
00000055596d2000       4       4       4 rw--- Mmap
0000007f8d02b000    1396     980       0 r-x-- libc-2.31.so
0000007f8d188000      60       0       0 ----- libc-2.31.so
0000007f8d197000      16      16      16 r---- libc-2.31.so
0000007f8d19b000       8       8       8 rw--- libc-2.31.so
0000007f8d19d000      12      12      12 rw---   [ anon ]
0000007f8d1b2000     136     120       0 r-x-- ld-2.31.so
0000007f8d1dc000       4       4       4 -w--- bar
0000007f8d1dd000       4       4       0 r---- Mmap.c
0000007f8d1de000       8       8       8 rw---   [ anon ]
0000007f8d1e0000       8       0       0 r----   [ anon ]
0000007f8d1e2000       4       4       0 r-x--   [ anon ]
0000007f8d1e3000       4       4       4 r---- ld-2.31.so
0000007f8d1e4000       8       8       8 rw--- ld-2.31.so
0000007fc3c4b000     132      12      12 rw---   [ stack ]
---------------- ------- ------- ------- 
total kB            1812    1192      80
pi@pi-logmans:~/week2/files $ cat bar
```

With the extra argument the process uses the flag `MAP_PRIVATE` and this does not allow other processes to see the changes to the mapping. Therefore `cat bar` returns nothing as the `cat` process cannot see anything.