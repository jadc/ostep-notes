## [[file-devices.pdf#page=1&selection=39,0,39,24&color=note| 36.1 System Architecture]]

> [!PDF|yellow] [[file-devices.pdf#page=1&selection=42,57,46,1&color=yellow|file-devices, p.1]]
> > CPU attached to the main memory of the system via some kind of memory bus

> [!PDF|yellow] [[file-devices.pdf#page=1&selection=48,59,54,3&color=yellow|file-devices, p.1]]
> > general I/O bus, which in many modern systems would be PCI

> [!PDF|yellow] [[file-devices.pdf#page=1&selection=58,35,72,26&color=yellow|file-devices, p.1]]
> > lower down are one or more of what we call a peripheral bus, such as SCSI, SATA, or USB. These connect slow devices to the system

> [!PDF|yellow] [[file-devices.pdf#page=1&selection=87,0,88,18&color=yellow|file-devices, p.1]]
> > The faster a bus is, the shorter it must be, and less room to plug devices into it.
> 
> ![[file-devices.pdf#page=2&rect=101,295,371,544&color=yellow|file-devices, p.2]]

> [!PDF|yellow] [[file-devices.pdf#page=1&selection=93,41,94,58&color=yellow|file-devices, p.1]]
> >  components that demand high performance (such as the graphics card) are nearer the CPU

> [!PDF|yellow] [[file-devices.pdf#page=2&selection=36,49,41,9&color=yellow|file-devices, p.2]]
> > one or more hard drives connect to the system via the eSATA interface
> 
> ![[file-devices.pdf#page=3&rect=82,335,336,553&color=yellow|file-devices, p.3]]

> [!PDF|yellow] [[file-devices.pdf#page=2&selection=70,48,71,20&color=yellow|file-devices, p.2]]
> > USB is used for low performance devices 

> [!PDF|yellow] [[file-devices.pdf#page=3&selection=44,0,48,1&color=yellow|file-devices, p.3]]
> > PCIe (Peripheral Component Interconnect Express)
> 
>  higher performance storage devices (such as NVMe persistent storage devices) are often connected here

## [[file-devices.pdf#page=3&selection=57,0,57,23&color=note|36.2 A Canonical Device]]

> [!PDF|yellow] [[file-devices.pdf#page=3&selection=67,0,68,33&color=yellow|file-devices, p.3]]
> > hardware must also present some kind of interface that allows the system software to control its operation
> 
> ![[file-devices.pdf#page=4&rect=110,463,348,552&color=yellow|file-devices, p.4]]

> [!PDF|yellow] [[file-devices.pdf#page=3&selection=84,0,86,41&color=yellow|file-devices, p.3]]
> > firmware (i.e., software within a hardware device)

## [[file-devices.pdf#page=4&selection=23,0,23,27&color=note|36.3 The Canonical Protocol]]

> [!PDF|yellow] [[file-devices.pdf#page=4&selection=25,38,41,10&color=yellow|file-devices, p.4]]
> > device interface is comprised of three registers: a status register, which can be read to see the current status of the device; a command register, to tell the device to perform a certain task; and a data register to pass data to the device, or get data from the device

> [!PDF|yellow] [[file-devices.pdf#page=4&selection=66,30,70,1&color=yellow|file-devices, p.4]]
> > repeatedly reading the status register; we call this polling

> [!PDF|yellow] [[file-devices.pdf#page=4&selection=72,9,72,53&color=yellow|file-devices, p.4]]
> > OS sends some data down to the data register

> [!PDF|yellow] [[file-devices.pdf#page=4&selection=73,43,74,35&color=yellow|file-devices, p.4]]
> >  multiple writes would need to take place to transfer a disk block

> [!PDF|yellow] [[file-devices.pdf#page=4&selection=75,0,78,20&color=yellow|file-devices, p.4]]
> > CPU is involved with the data movement = programmed I/O (PIO)

> [!PDF|yellow] [[file-devices.pdf#page=4&selection=79,13,82,4&color=yellow|file-devices, p.4]]
> > OS writes a command to the command register; doing so implicitly lets the device know that both the data is present and that it should begin working on the command

## [[file-devices.pdf#page=5&selection=34,0,34,42&color=note|36.4 Lowering CPU Overhead With Interrupts]]

> [!PDF|yellow] [[file-devices.pdf#page=5&selection=41,35,42,52&color=yellow|file-devices, p.5]]
> > OS can issue a request, put the calling process to sleep, and context switch to another task

> [!PDF|yellow] [[file-devices.pdf#page=5&selection=44,0,54,23&color=yellow|file-devices, p.5]]
> > When the device is finally finished with the operation, it will raise a hardware interrupt, causing the CPU to jump into the OS at a predetermined interrupt service routine (ISR) or more simply an interrupt handler

> [!PDF|yellow] [[file-devices.pdf#page=5&selection=56,63,57,62&color=yellow|file-devices, p.5]]
> > wake the process waiting for the I/O, which can then proceed as desired

> [!PDF|yellow] [[file-devices.pdf#page=5&selection=58,0,62,22&color=yellow|file-devices, p.5]]
> > Interrupts thus allow for overlap of computation and I/O

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=25,0,28,6&color=yellow|file-devices, p.6]]
> > Although interrupts allow for overlap of computation and I/O, they only really make sense for slow devices. Otherwise, the cost of interrupt handling and context switching may outweigh the benefits interrupts provide. 

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=34,0,37,72&color=yellow|file-devices, p.6]]
> > hybrid that polls for a little while and then, if the device is not yet finished, uses interrupts

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=45,1,49,6&color=yellow|file-devices, p.6]]
> > livelock, that is, find itself only processing interrupts and never allowing a user-level process to run and actually service the requests

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=54,49,55,65&color=yellow|file-devices, p.6]]
> > service some requests before going back to the device to check for more packet arrivals

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=62,19,63,68&color=yellow|file-devices, p.6]]
> > multiple interrupts can be coalesced into a single interrupt delivery, thus lowering the overhead of interrupt processing
> 
> Amortization of interrupt processing

## [[file-devices.pdf#page=6&selection=67,0,67,41&color=note|36.5 More Efficient Data Movement With DMA]]

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=71,0,72,35&color=yellow|file-devices, p.6]]
> > to transfer a large chunk of data to a device, the CPU is once again overburdened with a rather trivial task

> [!PDF|yellow] [[file-devices.pdf#page=6&selection=126,0,127,33&color=yellow|file-devices, p.6]]
> > When the copy is complete, the I/O begins on the disk and the CPU can finally be used for something else
> 
> The data is first copied to the device's controller (internal DRAM/SRAM buffer on device, recall the "data" register in the interface). Then, the device itself handles writing it to disk (e.g. in HDD, seeking to correct track, wait for rotation, then write sectors).

> [!PDF|yellow] [[file-devices.pdf#page=7&selection=29,2,31,36&color=yellow|file-devices, p.7]]
> > A DMA engine is essentially a very specific device within a system that can orchestrate transfers between devices and main memory without much CPU intervention
> 
> **CPU is involved** (heavily in programmed I/O, less so with DMA) in the _memory → controller_ phase.
> 
> The **disk hardware is involved** in the _controller → media_ phase, which is often much slower (seek times, rotation, flash program times).
> 
> From a scheduling point of view, the OS wants to run other processes while the disk hardware is doing its long-latency work.

> [!PDF|yellow] [[file-devices.pdf#page=7&selection=32,22,37,42&color=yellow|file-devices, p.7]]
> > To transfer data to the device, for example, the OS would program the DMA engine by telling it where the data lives in memory, how much data to copy, and which device to send it to. At that point, the OS is done with the transfer and can proceed with other work. When the DMA is complete, the DMA controller raises an interrupt, and the OS thus knows the transfer is complete

## [[file-devices.pdf#page=7&selection=94,0,94,34&color=note|36.6 Methods Of Device Interaction]]

> [!PDF|yellow] [[file-devices.pdf#page=7&selection=128,18,134,55&color=yellow|file-devices, p.7]]
> > explicit I/O instructions. These instructions specify a way for the OS to send data to specific device registers

> [!PDF|yellow] [[file-devices.pdf#page=8&selection=31,0,36,63&color=yellow|file-devices, p.8]]
> > memorymapped I/O. With this approach, the hardware makes device registers available as if they were memory locations. To access a particular register, the OS issues a load (to read) or store (to write) the address; the hardware then routes the load/store to the device instead of main memory

## [[file-devices.pdf#page=8&selection=41,0,41,43&color=note|36.7 Fitting Into The OS: The Device Driver]]
> [!PDF|yellow] [[file-devices.pdf#page=8&selection=84,0,86,58&color=yellow|file-devices, p.8]]
> > device driver, and any specifics of device interaction are encapsulated within

> [!PDF|yellow] [[file-devices.pdf#page=8&selection=92,23,94,19&color=yellow|file-devices, p.8]]
> > issues block read and write requests to the generic block layer, which routes them to the appropriate device driver, which handles the details
> 
> ![[file-devices.pdf#page=9&rect=76,417,337,553&color=yellow|file-devices, p.9]]

> [!PDF|yellow] [[file-devices.pdf#page=9&selection=25,1,38,26&color=yellow|file-devices, p.9]]
> > raw interface to devices, which enables special applications to directly read and write blocks without using the file abstraction. (e.g. `dd`)

> [!PDF|yellow] [[file-devices.pdf#page=9&selection=41,13,43,27&color=yellow|file-devices, p.9]]
> > if there is a device that has many special capabilities, but has to present a generic interface to the rest of the kernel, those special capabilities will go unused

> [!PDF|yellow] [[file-devices.pdf#page=10&selection=92,0,93,66&color=yellow|file-devices, p.10]]
> > An IDE disk presents a simple interface to the system, consisting of four types of register: control, command block, status, and error.
> 
> **View basic I/O sequence here as well.**