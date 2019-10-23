---
layout: post
title:  "Notes-About-WSL"
author: "eypidan"
---
# Windows Subsystem Linux

 

### History of Windows Subsystem

- Since its inception, Microsoft  Windows NT was designed to allow environment subsystems like Win32 to present a programmatic interface to applications without being tied to implementations details inside the kernel. This allowed the NT kernel to support POSIX, OS/2 and Win32 subsystems as its initial release. **Early subsystems were implemented as user mode modules** and subsystem API is implemented by PE/COFF executables, a set of libraries and services.
  - **POSIX**: POSIX defines the application programming interface (API), along with command line shells and utility interfaces, for software compatibility with variants of Unix and other operating systems.

  - **OS/2**:OS/2 was intended as a protected-mode successor of PC DOS. Notably, basic system calls were modeled after MS-DOS calls.

    - ![Os2logo.svg](/assets/250px-Os2logo.png)

  - **Win32**: The Win32 environment subsystem can run 32-bit Windows applications. It contains the console as well as text window support, shutdown and hard-error handling for all other environment subsystems.

    

- Later versions of subsystems replaced the POSIX layer to provide the Subsystem for Unix-based Applications (SUA). This composed of user mode components to satisfy:

  1. Process and signal management

  2. Terminal management

  3. System service requests and inter process communication

     

- Cygwin

  1. A POSIX-compatible environment that runs natively on Microsoft Windows.  
  2. Its goal is to allow programs of Unix-like systems to be recompiled and run natively on Windows with minimal source code modifications by providing them with the same underlying POSIX API they would expect in those systems. 
  3. Belong to Cygnus Solutions who belongs to RedHat.
  4. Consists two parts:
     
     1. A DLL as an API compatibility layer in the form of a C standard library providing a substantial part of the POSIX API functionality. 
     
        

- Now -> windows subsystems for linux. 

  - It was first made available in [Windows 10 Insider Preview](https://en.wikipedia.org/wiki/Windows_Insider) build 14316
  - **WSL**1 is a collection of components that enables native Linux ELF64 binaries to run on Windows. It contains both user mode and kernel mode components. It is primarily comprised of:
    - User mode session manager service that handles the Linux instance life cycle
    - Pico provider drivers (lxss.sys, lxcore.sys) that emulate a Linux kernel by translating Linux syscalls
    - Pico processes that host the unmodified user mode Linux (e.g. /bin/bash)
  - ![LXSS diagram](/assets/LXSS-diagram-1024x472.jpg)



### File System of WSL 

- File System support in WSL was designed to meet two goals.

  1.  Provide an environment that supports the full fidelity of Linux file systems 
  2.  Allow interoperability with drives and files in Windows 

- Linux File System

  ​	Linux abstracts file systems operations through the Virtual File System (VFS), which provides both an interface for user mode programs to interact with the file system (through system calls such as open, read, chmod, stat, etc.) and an interface that file systems have to implement. This allows multiple file systems to coexist, providing the same operations and semantics, with VFS giving a single namespace view of all these file systems to the user.

  ​	File systems are mounted on different directories in this namespace. For example, on a typical Linux system your hard drive may be mounted at the root, /, with directories such as /dev, /proc, /sys, and /mnt/cdrom all mounting different file systems which may be on different devices. Examples of file systems used on Linux include ext4( Fourth extended filesystem ), rfs(Remote FIle Sharing), FAT, and others.

  ​	VFS implements the various system calls for file system operations by using a number of data structures such as inodes, directory entries and files, and related callbacks that file systems must implement.

  > Inode :The inode is the central data structure used in VFS. It represents a file system object such as a regular file, directory, symbolic link, etc. An inode contains information about the file type, size, permissions, last modified time, and other attributes.

  ​    

- Windows File System

  ​	Windows generalizes all system resources into objects. These include not just files, but also things like threads, shared memory sections, and timers, just to name a few. All requests to open a file ultimately go through the Object Manager in the NT kernel, which routes the request through the I/O Manager to the correct file system driver. The interface that file system drivers implement in Windows is more generic and enforces fewer requirements. For example, there is no common inode structure or anything similar, nor is there a directory entry; instead, file system drivers such as ntfs.sys are responsible for resolving paths and opening file objects. 

- WSL File System

  ​    The Windows Subsystem for Linux must translate various Linux file system operations into NT kernel operations. WSL must provide a place where Linux system files can exist, with all the functionality required for that including Linux permissions, symbolic links and other special files such as FIFOs; it must provide access to the Windows volumes on your system; and it must provide special file systems such as ProcFs( proc filesystem ). 

  >  The **proc filesystem** (**procfs**) is a special filesystem in [Unix-like](https://en.wikipedia.org/wiki/Unix-like) operating systems that presents information about [processes](https://en.wikipedia.org/wiki/Process_(computing)) and other system information in a hierarchical file-like structure, providing a more convenient and standardized method for dynamically accessing process data held in the kernel than traditional [tracing](https://en.wikipedia.org/wiki/Tracing_(software)) methods or direct access to [kernel](https://en.wikipedia.org/wiki/Kernel_(computing)) memory. 
  
  - WSL File System's overall architecture:
  
  ![s](/assets/file-system-graphic-1024x547.png)
  
	When an application calls a system call, this is handled by the system call layer, which defines the various kernel entry points such as open, read, chmod, stat, etc. For these file-related system calls, the system call layer has very little functionality; it basically just forwards the call to VFS. 
  
	
	
	For operations that use paths (such as open or stat), VFS resolves the path using a directory entry cache. If an entry is not in the cache, it calls into one of several file system plugins to create an inode for the entry. These plugins provide inode operations like lookup, chmod, and others, similar to the inode operations used by the Linux kernel. When a file is opened, VFS uses the file system’s inode open operation to create a file object, and returns a file descriptor for that file object. System calls operating on the file descriptor (such as read, write or sync) call file operations defined by the file systems. This system is deliberately very close to how Linux behaves, so WSL can support the same semantics. 

### Difference Between WSL1 & WSL2

- WSL2
  1. File System: Use Virtual Hardwar Disk.
  2.  Has Linux Kernel
  3. Access NTFS very faster
  5. Full System Call Compatibility
      - Whereas WSL 1 used a translation layer that was built by the WSL team, WSL 2 includes its own Linux kernel with full system call compatibility. 
      - This introduces a whole new set of apps that you can run inside of WSL, such as Docker and more. 
  5. Increased file IO performance
      -  File intensive operations like git clone, npm install, apt update, apt upgrade, and more will all be noticeably faster. The actual speed increase will depend on which app you’re running and how it is interacting with the file system.  
        - May be because Linux kernel's system's call is better than the translation layer of WSL1. 
  6. Powered by Hyper-V, though in virtualization WSL2 takes few resources and can be fast to boot up.
- About Hyper-V
  -  The newest version of WSL uses Hyper-V architecture to enable its virtualization.  
  - **BAD THING** :  Some 3rd party applications cannot work when Hyper-V is in use, which means they will not be able to run when WSL 2 is enabled. Unfortunately, this does include VMware, and versions of VirtualBox before VirtualBox 6.

### Reference

- [Cygwin Wiki]( https://en.wikipedia.org/wiki/Cygwin)

- [Windows NT Wiki](https://en.wikipedia.org/wiki/Windows_NT)
- [WSL File System Blog](https://blogs.msdn.microsoft.com/wsl/2016/06/15/wsl-file-system-support)

- [WSL Overview Blog](https://blogs.msdn.microsoft.com/wsl/2016/04/22/windows-subsystem-for-linux-overview/)

- [POSIX Wiki](https://en.wikipedia.org/wiki/POSIX)
- [Procfs  Wiki](https://en.wikipedia.org/wiki/Procfs)

- [WSL2 Blog ]( https://docs.microsoft.com/en-us/windows/wsl/wsl2-about)

