# Fork, Signal and IPC
## Assignment 1: *Understanding Processes*
### a
```
main()
|--fork1
|----fork11
|------fork111
|----fork12
|--fork2
|----fork21
|--fork3
```

### b
```
main()
|--fork1
|--fork2
```

### c
```
main
|--fork1
|--fork2
|--fork3
```

## Assignment 2: *Working with Signals*
The program crashes fairly quickly. The following error is displayed:
``` s
Program received signal SIGSEGV, Segmentation fault.
0x000000555555089c in main (argc=1, argv=0x7ffffff4c8) at Signal.c:16
16	    *pointer=0;
```
Looking at the pointer, we see that it states the following:
``` s
1: pointer = (int *) 0x0
```

After some [research](https://stackoverflow.com/questions/1564372/what-causes-a-sigsegv), I concluded that the error for this segmentation fault was the dereferencing of a null pointer. The pointer is set to a non existing address. `*pointer` tries to access it. However, since it is null, it causes an segmentation error.

## Assignment 3: *Modify Signal.c*
To prevent the segmentation fault, we need to change the initial address of the pointer. I have done this by creating a temporary variable `temp` that will provide an address where the pointer is pointing to:
``` C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>

void signal_handler(int sig) {
    printf("Signal %d received.\n", sig);
    exit(0);
}

int main(int argc, char * argv[]) {
    int temp; 
    int *pointer= &temp;
    signal(SIGSEGV,signal_handler);
    printf("About to segfault:\n");
    *pointer=0;
    printf("Why didn't we crash?\n");
    return 1;
}
```

## Assignment 4: *SIGSEGV*
As the new signal, I have chosen the interrupt signal, `SIGINT`, with code 2. This is thrown when the user presses `ctrl + C`. The code looks as follows:
``` C
void signal_handler(int sig) {
    printf("Signal %d received.\n", sig);
    exit(0);
}

int main(int argc, char * argv[]) {
    int temp; 
    int *pointer= &temp;
    signal(SIGSEGV,signal_handler);
    signal(SIGINT, signal_handler);
    printf("About to segfault:\n");
    *pointer=10;
    printf("Value of pointer is %d.\n", temp);
    printf("Why didn't we crash?\n");
    sleep(5);
    return 1;
}
```

The output of the terminal is than the following:
```s
About to segfault:
Why didn't we crash?
^CSignal 2 received.
```


## Assignment 5: *Trying it all Together*
I changed the code in the following way:
``` C
void signal_handler(int sig) {
    printf("Signal %d received.\n", sig);
    exit(0);
}

int main(int argc, char * argv[]) {
    int temp; 
    int *pointer= &temp;

    fork();
    signal(SIGSEGV,signal_handler);
    signal(SIGINT, signal_handler);
    printf("About to segfault:\n");
    *pointer=10;
    printf("Value of pointer is %d.\n", temp);
    printf("Why didn't we crash?\n");
    sleep(5);
    return 1;
}
```

We fork before the signal handlers. Therefore, both the main process and the child process have a copy of the signal handler. Therefore, the output is the following:
``` s
About to segfault:
About to segfault:
Value of pointer is 10.
Why didn't we crash?
Value of pointer is 10.
Why didn't we crash?
^CSignal 2 received.
Signal 2 received.
```

As we can see, both the child process and parent process receive the signal

# Threads
## Assignment 6: *Basics of Threads*
The two compiled programs show the difference between single-threaded and multi-threaded. In the single-threaded output, you clearly see that first the `dash` function is called, and afterwards the `underscore` function. in the multi-threaded output, you see that the two threads are running at the same time. Here, the `dash` and the `underscore` function write at the same time to the output, resulting in a pattern between dashes and underscores intertwined. It is also notable that, when ran multiple times, the output is always different.


## Assignment 7: *Working with Threads*
Looking at the system calls, I identified the following code snippet:
```
set_tid_address(0x7fb3ed1110)           = 109558
set_robust_list(0x7fb3ed1120, 24)       = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7fb3e69b94, sa_mask=[], sa_flags=SA_SIGINFO}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7fb3e69c50, sa_mask=[], sa_flags=SA_RESTART|SA_SIGINFO}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
.
.
.
clone(child_stack=0x7fb3cedac0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSTEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARED, parent_tid=[109559], tls=0x7fb3cee8c0, child_tidptr=0x7fb3cee290) = 109559
```
The first few lines set a number of important items, such as the thread ID and the signal mask that needs to be applied. The final two rules actually create the thread similar as the fork operation from week 1.


Setting different `N` for the program, I concluded that the maximum numbers of threads that can run on the pi is 9084.


## **REVIEW** Assignment 8: *MyThread Timing*
Comparing the timing for 50 thread gives the following table:
| Timing | Java   | C      | Difference |
| ------ | ------ | ------ | ---------- |
| Real   | 0,546s | 0,022s | 0,542s     |
| User   | 0,997s | 0,001s | 0,996s     |
| Sys    | 0,179s | 0,028s | 0,151s     |


It is notable that C is faster than java on all points. From my understanding, this is because compiling C into machine code is way faster than doing the equivalent of java.e

## Assignment 9: *Power of Threads*
The idea that I had to multithread this program was to create a thread for all elements that need to be summed, and let them do that operation and add it to a central number.

The output of the program looks as follows:
```s
We have added. Total is now: 1.000000 
We have added. Total is now: 5.000000 
We have added. Total is now: 21.000000 
We have added. Total is now: 30.000000 
The result is: 30.000000 
```

where the program is:
``` C
#include <stdio.h>
#include <pthread.h>

double result = 0.0;
double v[] = {1,2,3,4,5,6,7}; 
double u[] = {1,2,3,4,5,6,7};
int n = 4;

void add(int i) {
    result += v[i] * u[i];
    printf("We have added. Total is now: %f \n" , result);
}

int main(int argc, char * argv[]) {
    pthread_t tid;
    for ( int i = 0; i < n ; i++) {
        pthread_create(&tid, NULL, add, i);
    }

    for ( int i = 0; i < n ; i++) {
        pthread_join(tid, NULL);
    }

    printf("The result is: %f \n", result);
    pthread_exit(NULL);
}
```
It sums the first 4 integers of both arrays using the adder function. For each multiplication and addition, a new thread is created. 


# Concurrency
## Assignment 10: *Parallel shell*
The command is: `cat index.html | sort | uniq -c | sort -rn | pr -2`

- `cat index.html` prints the content of the html file to the terminal
- `sort` sorts the line of this data file
- `uniq -c` only gets the unique lines in the date file and count the occurrence of each line
- `sort -rn` sorts the contents in reverse based on numeric values (highest to lowest)
- `pr -2` print the output to the terminal in two columns

Looking at the file and how it is edited, it looks as follows:
`index file` -> `sorted index file` -> `unique lines and their count` -> `sorted file based on counts` -> `print file`

It can be ran concurrent since since not all data is independent on the previous stage. For example, the counting lines is not dependent on the fact that the file is sorted as well. Afterwards, as soon as the highest count is found, it can be printed to the file whilst the previous stage is still sorting the rest of the file.

## Assignment 11: *Count.java*
Since there is no way to prevent the data race, the output is always different. Both threads try to write the variable simultaneously, and acquire random access to write the variable. Hence, the output of the program is different every time. However, regardless of the intermediate step, the final ctr is either `10` or  `-10`.


## Assignment 12: *Count with Semaphore*
Concurrency still seems possible. However, there are only two possible outcomes.
```
Output 1      Output 2
1	1       | -1 -1 
1	2       | -1 -2
1	3       | -1 -3
1	4       | -1 -4
1	5       | -1 -5
1	6       | -1 -6
1	7       | -1 -7
1	8       | -1 -8
1	9       | -1 -9
1	10      | -1 -10
-1	9       | 1	 -9
-1	8       | 1	 -8
-1	7       | 1	 -7
-1	6       | 1	 -6
-1	5       | 1	 -5
-1	4       | 1	 -4
-1	3       | 1	 -3
-1	2       | 1	 -2
-1	1       | 1	 -1
-1	0       | 1	 0
```
These two output shows that, once a thread secures the semaphore, it does not lose it's access to it and completes the entire process in one go. Either thread can acquire the semaphore first, yet ones it has the semaphore, it keeps it until it has completed.

The ctr of both outputs is always 0 and is therefore constant regardless of intermediate steps.

## **CONTINUE** Assignment 13: *Count in C and Java*
The results are not the same for both programs. For java, it is the same as the previous assignment. The C program is a bit less constant. It still happens more consistent than without the semaphore, however, it also occurs that one function is called 8 times, than the other function takes over, and than the initial function ends.

Some further investigation leaded me to teh fact that java has a more secure sephamore than C. In Java, it locks the object in a more secure way whereas C protects is a bit less. This allows other threads to interfere, but it speeds up the program massively.


## **TODO** Assignment 14: *Count and strace*



## Assignment 15: *Spinlock*
The `scheld_yield()` ensures that the thread yields and does not join the rest of the threads. Therefore, the output is a lot lower when this is activated. When it is activated, the output is `326236`. If not, the output is `98700581`. Since the threads yield within the second and not join the rest, the counter is a lot lower when it is used in the program.


## **TODO** Assignment 16: *Spinlock with processes (difficult)*



## Assignment 18: *ProdCons*
Both programs do the same. The output looks the following:
``` s
N = 1       | N = 4
    B[0]=0  |     B[0]=0
i=0 0=B[0]  |     B[1]=1
    B[0]=1  | i=0 0=B[0]
i=1 1=B[0]  | i=1 1=B[1]
    B[0]=2  |     B[2]=2
i=2 2=B[0]  |     B[3]=3
    B[0]=3  | i=2 2=B[2]
i=3 3=B[0]  | i=3 3=B[3]
    B[0]=4  |     B[0]=4
i=4 4=B[0]  |     B[1]=5
    B[0]=5  | i=4 4=B[0]
i=5 5=B[0]  | i=5 5=B[1]
    B[0]=6  |     B[2]=6
i=6 6=B[0]  |     B[3]=7
    B[0]=7  |     B[0]=8
i=7 7=B[0]  |     B[1]=9
    B[0]=8  | i=6 6=B[2]
i=8 8=B[0]  | i=7 7=B[3]
    B[0]=9  | i=8 8=B[0]
i=9 9=B[0]  | i=9 9=B[1]
```

`N` determines the buffer size of the program. This is the number of elements that can be loaded into the array before it needs to be unloaded. As can be seen, more elements are loaded at the time every time for `N=4`. This exchange is done between two threads. One of the threads is loading the data into the buffer whilst the other thread is getting the elements from the buffer.

## Assignment 18: *ProdCons initialisation*
The output looks as follows:
``` s
    B[0]=0
    B[1]=1
i=0 0=B[0]
i=1 1=B[1]
    B[1]=2
    B[1]=3
i=2 3=B[1]
    B[1]=4
ProdConsN: ProdCons.c:36: tcons: Assertion `i==k' failed.
Aborted
```


## Assignment 19: *ProdManyCons*
The output compares as follows:
``` s
Switch=false    | Switch=true
        B[0]=0  |           B[0]=0
        B[1]=1  |           B[1]=1
        B[2]=2  | 0=B[0]
        B[3]=3  | 1=B[1]
0=B[0]          |           B[2]=2
1=B[1]          |           B[3]=3
2=B[2]          | 2=B[2]
        B[0]=5  | 3=B[3]
        B[1]=6  |           B[0]=4
        B[2]=7  | 4=B[0]
3=B[3]          |           B[1]=5
5=B[0]          |           B[2]=6
         B[3]=8 |           B[3]=7
6=B[1]          |           B[0]=8
7=B[2]          | 5=B[1]
8=B[3]          | 6=B[2]
        B[0]=9  |           B[1]=9
        B[1]=4  | 7=B[3]
        B[1]=15 | 8=B[0]
9=B[0]          | 9=B[1]
15=B[1]         |           B[2]=15
7=B[2]          | 15=B[2]
        B[3]=16 |           B[3]=16
16=B[3]         | 16=B[3]
        B[0]=17 |           B[0]=17
17=B[0]         | 17=B[0]
        B[1]=18 |           B[1]=18
        B[1]=20 | 18=B[1]
        B[3]=21 |           B[2]=19
        B[2]=19 | 19=B[2]
20=B[1]         |           B[3]=10
19=B[2]         |           B[0]=20
21=B[3]         |           B[1]=21
        B[0]=22 | 10=B[3]
        B[2]=23 | 20=B[0]
22=B[0]         |           B[2]=22
20=B[1]         |           B[3]=23
10=B[2]         | 21=B[1]
        B[2]=10 | 22=B[2]
        B[0]=11 |           B[0]=24
        B[1]=12 | 23=B[3]
24=B[3]         | 24=B[0]
11=B[0]         |           B[1]=11
12=B[1]         | 11=B[1]
        B[3]=24 |           B[2]=12
10=B[2]         | 12=B[2]
        B[3]=13 |           B[3]=13
        B[0]=14 | 13=B[3]
13=B[3]         |           B[0]=14
14=B[0]         | 14=B[0]
```
When the switch was off, the following error appeared:
``` s
 ProdManyCons: ProdManyCons.c:32: tcons: Assertion `sum == (P*M-1)*P*M/2' failed.
Aborted
```

Looking at the code, the switch is responsible for switching on and off a semaphore. This protects the buffer in a way that only one thread at the time can write/read from the buffer. 
This is why when  it is not turned on, the code can error. In this case, the sum assertion did not add up. This can be backtraced to the numbers that are read from the thread. as can be seen, if the semaphore is not activated,
the numbers appear to mostly be in order, but can change order very quickly. With the semaphore turned on, this did not happen. Since it does not happen in order, the sum is different than what it should be. Therefore, the code errors on this assertion.

## Assignment 20: *ProdCons sem_wait Spaces*
When removed, the output is the following:
```s
    B[0]=0
    B[1]=1
    B[0]=2
    B[1]=3
    B[0]=4
    B[1]=5
    B[0]=6
    B[1]=7
    B[0]=8
    B[1]=9
i=0 0=B[0]
i=1 9=B[1]
```

As can be seen, the buffer is overflowing, by writing elements to occupied spaces. Some further investigation into `sem_wait()` learned me that this functions waits until the semaphore can be decreased.
If this is not possible, it will wait until this is possible. Since the semaphore is only increased when an element is read to the buffer, the functions writes all numbers directly to the buffer, causing on overflow.


## Assignment 21: *ProdCons sem_wait Elements*
The output when `sem_wait(&Elements)` is removed is the following:
``` s
    B[0]=0
    B[1]=1
i=0 0=B[0]
i=1 1=B[1]
    B[0]=2
    B[1]=3
i=2 2=B[0]
i=3 3=B[1]
    B[0]=4
    B[1]=5
i=4 4=B[0]
i=5 5=B[1]
    B[0]=6
    B[1]=7
i=6 6=B[0]
i=7 7=B[1]
i=8 6=B[0]
    B[0]=8
    B[1]=9
```
After this, it aborts with the following error:
``` s
NoElementProd: ProdCons.c:36: tcons: Assertion `i==k' failed.
Aborted
```
The error occurs since the buffer is not empty but no more elements are being written to the second thread. This is done since the semaphore does not count lower, but there are still items inside 
the buffer. Therefore, the highest number that is loaded from the buffer, `i`, is not equal to the numbers loaded into the buffer.
# Memory
## Assignment 22: *ProcessLayout*
`./ProcessLayout3` has two more [anon] calls in the process layout. Some [research](https://unix.stackexchange.com/questions/677006/what-is-anon-pages-in-memory) taught me that these calls are to anonymous memory. This is specific for the thread and the process and cannot be shared. Next to these calls, the rest of the calls were identical for both process layouts. Looking at this, I conclude that `./ProccesLayout2` had relative more calls to shared memory.


# Virtual Memory
## Assignment 23: *Getrusage*
The output looks very different for both programs
```
|lock off                   | Lock on
| cnt=       0, flt=101     | cnt=       0, flt=109
| cnt=     138, flt=102     | 
| cnt=    1138, flt=103     | 
| cnt=    2138, flt=104     | 
| cnt=    3138, flt=105     | 
| cnt=    4138, flt=106     | 
| cnt=    5138, flt=107     | 

```
Looking at the source code, the main difference between two executions is the usage of `mlock`. According to the manual page, `mlock` locks a piece of the virtual memory addresses and prevents it to being paged into the swap memory. To my understanding, the lock is responsible for pre-loading pages. This means that a number of physical addresses are already loaded into the virtual memory, lowering the number of page faults significantly. When this is not done, most pages have to be looked up in memory since they cause a page fault to occur. 

## Assignment 24: *Getrusage direct*
The main difference with the other output is that the number of page faults is higher when `DIRECT` is used and the lock is off. When the lock is on, no page fault is created. This seems to be caused by using a different index on the arrays when looking for the page. With the lock on, no page faults are created, hence all pages can be loaded into the virtual memory.


## Assignment 25: *Mmap*
The program loads the first file into the virtual memory by creating a mapping. The contents of the second file are then loaded trough this mapping. Therefore, the source code nevers enters the memory completely, only the mapping is loaded.


## Assignment 26: *Mmap with argument*
Comparing the files, I notice that `foo` contains the source code of the document, but `bar` is empty. This is explained by the flag `MAP_PRIVATE`. This flag is activated when more than 3 arguments are inputted. It prevents other processes from reading this mapping from virtual memory. Therefore, the process copying from this address to the `bar` document cannot read from this memory, hence nothing is copied into the file. For the `foo` document, the flag is set the `MAP_SHARED`, allowing other processes to read from the virtual memory address.