> [!PDF|yellow] [[file-journaling.pdf#page=1&selection=34,44,57,5&color=yellow|file-journaling, p.1]]
> **Crash-Consistency Problem**
> 
> > Imagine you have to update two on-disk structures, A and B, in order to complete a particular operation. Because the disk only services a single request at a time, one of these requests will reach the disk first (either A or B). If the system crashes or loses power after one write completes, the on-disk structure will be left in an inconsistent state

