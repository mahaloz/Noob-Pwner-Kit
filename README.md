# Noob-Pwner-Kit
## Purpose
This kit was created to help me, and anyone else who is trying to takeup the dreaded
art of binary pwning, find useful links to tools, challs, and tutorials. I have found
many of these places from experiening my noob-ness first hand in CTF challenges. I
hope you enjot the kit.

## Background
Binary Pwning can be devided into a few parts that tend to intertwine out in the wild.
1. Reverse Engineering
    1. Static Analysis
    2. Dynamic Analysis
    3. Weird Love Childs
2. [Heap Exploitation](#heap-exploitation)
3. Stack Exploitation
4. [Shellcoding](#shellcoding)
5. Network Program Exploitation
6. Misc

## Heap Exploitation
### Heap Layout
To understand how to beat the heap, you got to understand where it lives.
The heap exsists under the .data allocation area (the place the actual program code
is located). It grows down towards the Stack, while the Stack grows up towards it.
So, the heap is located between the .data and the Stack. The heap is mainly controled byglibc. 

The functions used to control the heap are sbrk(), mmap(), and malloc() & free(). 
sbrk() and mmap() actually address memory once the initial heap is done, which
complicates the entire process and makes it easier to use malloc().

ptmalloc2 is something that abstracts the tracking of the heap (i.e. its size and 
location). This abstraction is broken into three layers:
1. arena: Top, reliant on sbrk() and mmap().
2. heap: single contiguous memory region recording each mmap() we call to grow the arena. 
3. chunk: Bottom, free places, great place to attack.

NOTE: for more info see [this](https://medium.com/@c0ngwang/the-art-of-exploiting-heap-overflow-part-4-4f1140585210)

### Chunk and malloc()
Each chunk has a header which says if its free or not for use. When malloc() is used, the size of the header is added to whatever the size requested is. The header is placed at the beginning of the chunk, **easy to edit if you know the address of the memory**. 
The header should look something like this:
```c
    struct chunk {
    INTERNAL_SIZE_T      mchunk_prev_size;
    INTERNAL_SIZE_T      mchunk_size;
    struct malloc_chunk* fd;
    struct malloc_chunk* bk;
    }
```
Because each header should be linked to the last chunk, we have the linkedlist apporach.
#### Sorting these chuncks
When malloc() is activated we use chunks. Chunks are sorted in a few ways.
1. Fast Bins
    It contains 10 types of chunks based on their size malloc() in bytes.
    ![alt text](https://cdn-images-1.medium.com/max/1600/1*vKesMDWlcOf0EHRMJKNIjg.png)

    They are sorted in LIFO (Last In, First Out).
2. Normal Bins
    1. Unsorted: Only for temp bins being moved to other normal bins. Doubly linked.
    2. Small: 62 types just like fast starting at 8-bytes. Doubly linked, FIFO.
    3. Large: Chunks between 512 bytes - 128k bytes.
Larger than that an mmap() does it directly.

### Abusing linked chunks




### Resources
(blackhat-pres)[https://www.blackhat.com/presentations/bh-usa-07/Ferguson/Presentation/bh-usa-07-ferguson.pdf] - condensed info on fastbins and such
 
## Shellcoding

Shellcoding must be used every now-and-then to exploit a system in a customizable, yet highly restricted, byte code.
Shellcoding consists of:
1. Ideallizing your C code that is wanted for execution
2. Convert your C code into assembly
3. Remove Nullbytes (most times) from your assembly
4. Assure your shellcode is small (or big) enough to fit in your designated buffer
5. Convert assmbly into bytes [see 3].
6. Get those bytes into your exploited program

Each section consists of roadblocks and tools that are needed to get the job done that often circle each step,
similar to the scientific method.

### C Code Creation (CCC)
CCC is the first step to making a *killer* of a exploit.
This step is arbitrarily hard to explain becasue each CCC is different in the scenario.
For your convience, a shellcode example is provided:

#### Open, Read, Write
Consider you have program with which you can inject shellcode to execute. For this problem, the functions we are allowed to use
are Open, Read, and Write. The flags name is "flag".
The C code would look something like this:
```C
int fd = open("flag", O_RDONLY) //open the file
char* buf;
read(fd, buf, 1024) //assuming the flag is no bigger than 1024
write(1, buf, 1024) //write to stdout
```
the next part is translating this to asm.

### Assembling maddness
Continuing on our C code example we need to convert these system calls into assembly.
Luckily, assembly has an easy way to call system calls, which each has a value in hex.
The x86 table can be found here: [x86 Table](https://syscalls.kernelgrok.com/)
The x64 table can be found here: [x64 Table](https://filippo.io/linux-syscall-table/)

The above values will be used when we call the syscalls in an assembler convention.
For the continued example, we will assume x64 architecture is used. Calling conventions can be found here: [x86 & x64 Calling Conventions](https://en.wikipedia.org/wiki/X86_calling_conventions)

Starting with the open function, we need to start passing parameters to registers.

 







