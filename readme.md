# Virtual-Memory-Implementation-in-MOS(Multiprogramming Operating System)

## Simulation of a Paging System:

In this simulation, consider a *memory location* to be an atomic unit, that is, the smallest possible unit we care to consider. Thus, in a system with a page size of 2, there are two memory locations on each page.

The program's main memory holds **512** memory locations

Two files are supplied for the purpose of example:
- plist
  - Contains a list of programs that will be loaded into main memory.
  - Each line has the format `(pid, total # of memory locations)` which specifies the total number of memory locations needed by each program
- ptrace
  - Contains a deterministic series of memory accesses which emulate a real system's memory usage.
  - Each line has the format `(pid, referenced memory location)`, which specifies the memory location requested by the program.
 
##  Running the programm:
1. Simulate a paging system
2. Implement the three different page replacement algorithms
3. Handle a variable page size specified by the user
4. Implement both demand and pre-paging
5. Record the number of page swaps that occured during a run


## Plist and Initial Loading

Each page in each page table will have a name or number (which is *unique* with respect to all pages accross all page tables) so it can quickly determine whether it is present in main memory or not. The size of each page table is decided by the number of pages in the program. It is calculated by dividing the total number of memory locations of the program (found in `plist`) by the page size (from input parameters).

Thus, page tables are represented by the following data structure:

| Page number | Valid bit         | Last time accessed |
|-------------|-------------------|--------------------|
| N1          | 0 (not in memory) | T1                 |
| N2          | 1 (in memory)     | T2                 |
| ...         | ...               | ...                |

Once we have the page tables, it will perform a default loading of memory before starting to read the pages as indicated in `ptrace`. That is, we will load an initial resident set of each program's page table into main memory. *The main memory is divided equally among all programs in plist*. If the program doesn't have enough pages for its default load, it will leave its unused load blank. After initializing memory allocation, it will update the page tables (i.e. set valid bit of corresponding pages in table to 1) according to the page assignment. *If it doesn't divide evenly, it will keep the leftover memory*.

#### ptrace

Finally, the program will begin reading in `ptrace`. Each line of this file represents a memory location request within a program. It will need to translate this location into the unique page number that it has stored in the page tables made later, and determine if the requested page is in memory or not. If it is, it will simply continue with the next command in ptrace. If not, it will record that a page swap was made, and initiate a page replacement algorithm. **For each program, the pages to be replaced are picked from those pages allocated to itself (which is called local page allocation policy).**

![](paging_model.png?raw=true)
