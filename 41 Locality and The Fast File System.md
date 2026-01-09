> [!PDF|yellow] [[file-ffs.pdf#page=1&selection=42,23,46,62&color=yellow|file-ffs, p.1]]
> >  the old UNIX file system treated the disk like it was a random-access memory; data was spread all over the place

> [!PDF|yellow] [[file-ffs.pdf#page=2&selection=23,64,25,48&color=yellow|file-ffs, p.2]]
> > a logically contiguous file would be accessed by going back and forth across the disk, thus reducing performance dramatically

> [!PDF|yellow] [[file-ffs.pdf#page=2&selection=75,26,77,14&color=yellow|file-ffs, p.2]]
> > E gets spread across the disk, and as a result, when accessing E, you don’t get peak (sequential) performance from the disk.

> [!PDF|yellow] [[file-ffs.pdf#page=2&selection=85,13,91,18&color=yellow|file-ffs, p.2]]
> > disk defragmentation tools help with; they reorganize ondisk data to place files contiguously and make free space for one or a few contiguous regions

> [!PDF|yellow] [[file-ffs.pdf#page=2&selection=94,65,101,28&color=yellow|file-ffs, p.2]]
> > Smaller blocks were good because they minimized internal fragmentation (waste within the block), but bad for transfer as each block might require a positioning overhead to reach it

> [!PDF|yellow] [[file-ffs.pdf#page=3&selection=59,2,65,20&color=yellow|file-ffs, p.3]]
> > A single cylinder is a set of tracks on different surfaces of a hard drive that are the same distance from the center of the drive;

> [!PDF|yellow] [[file-ffs.pdf#page=4&selection=17,52,22,12&color=yellow|file-ffs, p.4]]
> >  organize the drive into block groups, each of which is just a consecutive portion of the disk’s address spac

> [!PDF|yellow] [[file-ffs.pdf#page=4&selection=34,8,35,75&color=yellow|file-ffs, p.4]]
> > by placing two files within the same group, FFS can ensure that accessing one after the other will not result in long seeks across the disk

> [!PDF|yellow] [[file-ffs.pdf#page=4&selection=39,38,41,21&color=yellow|file-ffs, p.4]]
> > within each group, e.g., space for inodes, data blocks, and some structures to track whether each of those are allocated or free

> [!PDF|yellow] [[file-ffs.pdf#page=5&selection=94,3,95,44&color=yellow|file-ffs, p.5]]
> > allocate the data blocks of a file in the same group as its inode, thus preventing long seeks between inode and data

> [!PDF|yellow] [[file-ffs.pdf#page=5&selection=96,11,110,20&color=yellow|file-ffs, p.5]]
> > places all files that are in the same directory in the cylinder group of the directory they are in. Thus, if a user creates four files, /a/b, /a/c, /a/d, and /b/f, FFS would try to place the first three near one another (same group)

> [!PDF|yellow] [[file-ffs.pdf#page=9&selection=123,47,126,12&color=yellow|file-ffs, p.9]]
> > process of reducing an overhead by doing more work per overhead paid is called amortization

> [!PDF|yellow] [[file-ffs.pdf#page=11&selection=55,27,62,25&color=yellow|file-ffs, p.11]]
> > As the file grew, the file system will continue allocating 512-byte blocks to it until it acquires a full 4KB of data. At that point, FFS will find a 4KB block, copy the sub-blocks into it, and free the sub-blocks for future use

> [!PDF|yellow] [[file-ffs.pdf#page=11&selection=70,27,72,55&color=yellow|file-ffs, p.11]]
> > buffer writes and then issue them in 4KB chunks to the file system, thus avoiding the sub-block specialization entirely in most cases

> [!PDF|yellow] [[file-ffs.pdf#page=11&selection=74,43,76,69&color=yellow|file-ffs, p.11]]
> > before SCSI and other more modern device interfaces), disks were much less sophisticated and required the host CPU to control their operation in a more hands-on way

> [!PDF|yellow] [[file-ffs.pdf#page=11&selection=79,58,82,60&color=yellow|file-ffs, p.11]]
> > FFS would first issue a read to block 0; by the time the read was complete, and FFS issued a read to block 1, it was too late: block 1 had rotated under the head and now the read to block 1 would incur a full rotation

> [!PDF|yellow] [[file-ffs.pdf#page=11&selection=86,54,92,15&color=yellow|file-ffs, p.11]]
> > for a particular disk how many blocks it should skip in doing layout in order to avoid the extra rotations

> [!PDF|yellow] [[file-ffs.pdf#page=11&selection=92,17,97,46&color=yellow|file-ffs, p.11]]
> > this technique was called parameterization, as FFS would figure out the specific performance parameters of the disk and use those to decide on the exact staggered layout scheme

