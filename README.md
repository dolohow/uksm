# UKSM

UKSM developer Nai Xia is not active anymore and @pfactum abandoned the patchset due to lacking active maintainer.
This is my attept to resync UKSM patchset to the latest stable kernels.

# What is it?

The Ultra Kernel Samepage Merging feature
----------------------------------------------

Ultra KSM. Copyright (C) 2011-2012 Nai Xia

This is an improvement upon KSM. Some basic data structures and routines
are borrowed from ksm.c .

Its new features:
1. Full system scan:
     It automatically scans all user processes' anonymous VMAs. Kernel-user
     interaction to submit a memory area to KSM is no longer needed.

2. Rich area detection:
     It automatically detects rich areas containing abundant duplicated
     pages based. Rich areas are given a full scan speed. Poor areas are
     sampled at a reasonable speed with very low CPU consumption.

3. Ultra Per-page scan speed improvement:
     A new hash algorithm is proposed. As a result, on a machine with
     Core(TM)2 Quad Q9300 CPU in 32-bit mode and 800MHZ DDR2 main memory, it
     can scan memory areas that does not contain duplicated pages at speed of
     627MB/sec ~ 2445MB/sec and can merge duplicated areas at speed of
     477MB/sec ~ 923MB/sec.

4. Thrashing area avoidance:
     Thrashing area(an VMA that has frequent Ksm page break-out) can be
     filtered out. My benchmark shows it's more efficient than KSM's per-page
     hash value based volatile page detection.


5. Misc changes upon KSM:
     * It has a fully x86-opitmized memcmp dedicated for 4-byte-aligned page
       comparison. It's much faster than default C version on x86.
     * rmap_item now has an struct *page member to loosely cache a
       address-->page mapping, which reduces too much time-costly
       follow_page().
     * The VMA creation/exit procedures are hooked to let the Ultra KSM know.
     * try_to_merge_two_pages() now can revert a pte if it fails. No break_
       ksm is needed for this case.

6. Full Zero Page consideration(contributed by Figo Zhang)
   Now uksmd consider full zero pages as special pages and merge them to an
   special unswappable uksm zero page.

# Credits

Ultra KSM. Copyright (C) 2011-2012 Nai Xia
