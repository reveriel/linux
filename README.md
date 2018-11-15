# KSM

KSM(Kernel Samepage Merging). Try to improve it. Based on
[PKSM](code.google.com/archive/p/pksm).
Kernel v3.18-rc7

## Possible plans

- delay
- hash adjustable.
- test uksm
- hash table instead of rbtree
- same virtual address
- page cache


## Background, KSM

KSM, corresponding file [`ksm.c`](mm/ksm.c), merges pages with same contents.
KSM is enabled by setting `CONFIG_KSM=y`. It can saves a lot of memory in
Virutal Machine Hypervisor.

You can mark a pieces of memory as "Mergeable" using syscall `madvice`, then
there is a kthread `[ksmd]` that scan all anonymous pages in the Mergeable 
memory area. When found two pages equal, `ksmd` sets two pages sharing one 
physical page by using COW(Copy on Write) mechanism.

The main data struct is two red black tree used to find equal pages. Just 
like we when we want to find repeating numbers in a large array, sort it or 
insert to a hashtable are the two most obvious way.

The hashtable method was used in VMware's Virtual Machine products. And a
patent claim its intellectual property. So Linux community pick the other
way --- sort the array.

> Note: Sort a constantly changing array


### Delay

pksm has three queues as worklists.
- new anon page list, or new list.
    	- every new anon page is added to this list.
- rescan anon page list, or rescan list.
    	- pages that failed to merge, or removed from unstable tree
- delete anon page list, or del list.




