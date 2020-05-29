This is the handout directory for the Cache Lab.
************************
# Running the Cache Simulator
The reference simulator takes the following command-line arguments:

    Usage: ./csim-ref [-hv] -s -E -b -t

-h: Optional help flag that prints usage info
-v: Optional verbose flag that displays trace info
-s : Number of set index bits (S = 2s is the number of sets)
-E : Associativity (number of lines per set)
-b : Number of block bits (B = 2b is the block size)
-t : Name of the valgrind trace to replay

For example: running

    linux> ./csim-ref -s 4 -E 1 -b 4 -t traces/yi.trace
Should yield

    hits:4 misses:5 evictions:3

With the verbose mode on, it should yield

    linux> ./csim-ref -v -s 4 -E 1 -b 4 -t traces/yi.trace
    L 10,1 miss
    M 20,1 miss hit
    L 22,1 hit
    S 18,1 hit
    L 110,1 miss eviction
    L 210,1 miss eviction
    M 12,1 miss eviction hit
    hits:4 misses:5 evictions:3

### Valgrind Trace Files

The `traces` subdirectory of the handout directory contains a collection of *reference trace files* that we will use to evaluate the correctness of the cache simulator you write in Part A. The trace files are generated by a Linux program called valgrind. For example, typing

    linux> valgrind --log-fd=1 --tool=lackey -v --trace-mem=yes ls -l

the command line runs the executable program “ls -l”, captures a trace of each of its memory accesses in the order they occur, and prints them on `stdout`.
`Valgrind` memory traces have the following form:

    I 0400d7d4,8
     M 0421c7f0,4
     L 04f6b868,8
     S 7ff0005c8,8

 Each line denotes one or two memory accesses. The format of each line is

    [space]operation address,size

 The operation field denotes the type of memory access: “I” denotes an instruction load, “L” a data load, “S” a data store, and “M” a data modify (i.e., a data load followed by a data store). There is never a space before each “I”. There is always a space before each “M”, “L”, and “S”. The address field specifies a 64-bit hexadecimal memory address. The size field specifies the number of bytes accessed by the operation.

# Running the autograders:

************************

Before running the autograders, compile your code:

linux> make

### Check the correctness of your simulator:

linux> ./test-csim

Check the correctness and performance of your transpose functions:

linux> ./test-trans -M 32 -N 32

linux> ./test-trans -M 64 -N 64

linux> ./test-trans -M 61 -N 67

Check everything at once (this is the program that your instructor runs):

linux> ./driver.py

******

Files:

******

# You will modifying and handing in these two files

csim.c Your cache simulator

trans.c Your transpose function

# Tools for evaluating your simulator and transpose function

Makefile Builds the simulator and tools

README This file

driver.py* The driver program, runs test-csim and test-trans

lab3.c Required helper functions

lab3.h Required header file

csim-ref* The executable reference cache simulator

test-csim* Tests your cache simulator

test-trans.c Tests your transpose function

tracegen.c Helper program used by test-trans

traces/ Trace files used by test-csim.c
