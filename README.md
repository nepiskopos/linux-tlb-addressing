# Linux addressing
Translation of logical addresses to physical addresses, using translation lookaside buffer.

## Description
This repository contains a very simple simulation of on-demand paging insipred by [Virtual Memory management](https://en.wikipedia.org/wiki/Virtual_memory) for Linux.

This simulation aims at performing virtual to physical memory address translation, utilizing a [translation lookaside buffer (TLB)](https://en.wikipedia.org/wiki/Translation_lookaside_buffer).

We create a virtual memory space of <img src="https://render.githubusercontent.com/render/math?math=2^16"> addresses, a TLB of 16 entries, and a single-layer page table (we assume that the page table is of unlimited space).

Then, we read a series of logical addresses from the [addresses file](/files/addresses.txt) in which all addresses are stored as unsigned integer numbers and we convert then into physical addresses.

Every logical address consists of:
* an 8-bit page number
* an 8-bit offset (within the page)

After an address is read from the file, we search for it in the TLB. We consider the 2 following cases:
* <b>Page fault</b>. In this case, if the address does not exist in the page table, we open the [content file](/files/BACKING_STORE.bin), we read the corresponding page, we store it both in the TLB and the page table and we extract the signed byte which is stored in this physical address.
* <b>Page hit</b>. In this case, we just extract the signed byte which is stored in this physical address.

The TLB is updated using the [Least Recently Used (LRU)](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU)) policy.
