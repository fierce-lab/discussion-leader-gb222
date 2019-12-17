# Review

## Info

Reviewer Name: Gary Baginski
Paper Title: Virtual Memory, Processes, and Sharing in MULTICS/The Multics Virtual Memory: Concepts and Design

## Summary

As information sharing becomes more commonplace and important, MULTICS offers an alternative to the common approach to sharing using file I/O.  Multics' segmentation scheme makes all information directly addressable while enabling the sharing of segments across users and programs.

### Problem and why we care

At the time of this paper, computers were terminals that had large virtual memories and were often used by many people at once.  Many applications for these computers share large amounts of information across users and across programs, and need to do so in a secure and controlled way.  An improved sharing scheme would be a great optimization to these systems.

### Gap in current approaches

Current approaches to sharing data across programs involves shared files.  The Multics paper cites the CTSS system as an example, in which to share a file between multiple users, a copy of that file would be placed in a buffer in each user's core image through a request to the filesystem.  Any modification made to the copy is not reflected in the original file until another request is made to the filesystem.  For a number of reasons, this approach is not practical in the nonsegmented systems of the time without hardware support; mainly, it would be impossible to discern crucial data about information in the core image, i.e. its length and access rights.

The papers also note some problems in most segmented systems: in systems where user programs open a file via a system call that allocates a segment descriptor for that file, there is typically a limited number of segment descriptors available to the program.  This can require programs to free segment descriptors when they have run out, and may prevent programs from directly accessing all the files it needs simultaneously

### Hypothesis, Key Idea, or Main Claim

The Multics paper proposes two design goals for a segmentation scheme that would provide "a generalized basis for the direct accessing and sharing of online information" : 1) that all "on-line" information in the system be directly addressable by a processor, and 2) that access to all information in the system can be controlled at each reference.

Multics' segmentation addresses the problems it mentions about other segmented systems by having a "sufficiently large" number of segment descriptors available to each user program so that one can expect to not run out and eliminating the need for buffering schemes.  Additionally, Multics manages allocation and deallocation of segment descriptors, so opening a file becomes much like requesting a segment of core memory from the kernel.  As a result, Multics has no traditional notion of a file; all information in the system is accessible as segments.  Now, access to all information, including files and memory, is subject to the access rights specified in the segment descriptor at each access.  

### Method for Proving the Claim

As mentioned above, Multics makes information in secondary storage directly addressable through its segmentation scheme.  Its virtual memory is large enough to consist of all of core memory and files with which a segment descriptor is associated.  As access to all information in segments is subject to its access rights, it's evident that key design goal #2 is satisfied.  However, key design goal #1 is worded a little more nebulously.  It's worth noting that while Multics takes care of segment descriptor management for files, a file is only provided with a segment descriptor "upon first reference to the information by a user program."  This step is prerequisite to information in secondary storage being directly addressable by a processor.

### Method for evaluating

Neither paper includes an evaluation section nor any explicit measurement of their performance.  The correctness of their solution should be evident from their implementation, but their performance is not evaluated in these papers.

Although the papers explain the current state of computing and identify an area for improvement, whether or not Multics' design goals represent an actual improvement to shared computing is also open to interpretation.

### Contributions: what we take away

My takeaway from these papers is that files are not the solution to everything.  Information sharing across users and programs is commonplace, and there's no reason for files or information copying to be necessary for sharing.  Security policies should be unified when possible; this just makes things simpler and eliminates the need to interface between different systems.

## Pros (3-6 bullets)

* Information sharing without copying is theoretically a great performance improvement over sharing through files.

* By accessing files through segments, files inherit their access protection scheme.  All information is protected by policies that can be modified by the owner of the information.

* All segments belong to the hierarchical directory structure one would expect from a file system.  Main memory can be organized much like the disk is in a conventional filesystem.  This can make the management of shared memory easier.

## Cons (3-6 bullets)

* Common file I/O abstractions are unavailable

* All segments belong to the hierarchical directory structure one would expect from a file system.  This could be seen as burdensome when main memory does not need to be shared.

* The Honeywell 645 on which Multics was designed to run is prohibitively expensive.  For context, its successor the 645F, also known as the Honeywell 6180, went for about $7 million

## Details Comments, Observations, Questions

* What are some of the implications of making all segments - including those of main memory - part of a hierarchical directory structure?

* What does addressing look like?  The paper states, "the user program can now address any word of the corresponding linear memory by the pair (name, i) where 'name' is the symbolic name of the segment and 'i' is the word number in the linear memory."  Is this how all memory referencing happens?  What implications does this have for development, compilation and assembly?

