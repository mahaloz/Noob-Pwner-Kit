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
2. Heap Exploitation
3. Stack Exploitation

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





