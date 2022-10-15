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



# Concurrency

## Assignment 10: *Parallel shell*



## Assignment 11: *Count.java*



## Assignment 12: *Count with Semaphore*



## Assignment 13: *Count in C and Java*



## Assignment 14: *Count and strace*



## Assignment 15: *Spinlock*



## Assignment 16: *Spinlock with processes (difficult)*



## Assignment 18: *ProdCons*



## Assignment 19: *ProdCons initialisation*



## Assignment 20: *ProdManyCons*



## Assignment 21: *ProdCons sem_wait Spaces*



## Assignment 22: *ProdCons sem_wait Elements*


# Memory

## Assignment 23: *ProcessLayout*


# Virtual Memory

## Assignment 24: *Getrusage*



## Assignment 25: *Getrusage direct*

## Assignment 26: *Mmap*


## Assignment 27: *Mmap with argument*
