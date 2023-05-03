Download Link: https://assignmentchef.com/product/solved-cs342-operating-systems-project-4-very-simple-file-system
<br>
<strong> </strong>

<strong> </strong>

<strong>Assignment </strong>




In this project you will implement a very simple file system called <strong>vsfs</strong>. The file system will be implemented as a library (<strong>libvsimplefs.a</strong>) and will store files in a virtual disk. The virtual disk will be a simple regular Linux file of some certain size. An application that would like to create and use files will be linked with the library. We will assume that the virtual disk will be used by one process at a time. When the process using the virtual disk terminates, then another process that will use the virtual disk can be started. With this assumption, we will not worry about race conditions.




<h1>1.1 Interface</h1>




The library will implement the following functions that can be called by an application. These functions will be implemented in a file called <strong>vsimplefs</strong>.<strong>c</strong>. The prototypes of these functions will be included in a header file called <strong>vsimplefs.h</strong>. In vsimplefs.c, you can  implement  and use some additional functions that will not be called by applications directly. You can also define as many structures and variables as you wish.




int <strong>create_format_vdisk</strong> (char *vdiskname, int m).

This function will be used to create and format a virtual disk. The virtual disk will simply be a Linux file of certain size. The name of the Linux file is vdiskname. The parameter m is used to set the size of the filee. Size will be 2^m bytes. The function will  initialize/create a vsfs file system on the virtual disk (this is high-level formatting of the disk).  On-disk file system structures (like superblock, FAT, etc.) will be  initialized on the virtual disk. If success, 0 will be returned. If error, -1 will be returned.




int <strong>vsfs_mount</strong> (char *vdiskname).

This function will be used to mount the file system, i.e., to  prepare the file system to be used. This is a simple operation.  Basically, it will open the regular Linux file (acting as the virtual disk) and obtain an integer file descriptor.  Other operations in the library  will use this file descriptor. This descriptor will be a global variable in the library. If success, 0 will be returned; if error, -1 will  be returned.




int <strong>vsfs_umount</strong> ().

This function will be used to unmount the file system: flush the cached data to disk and close the virtual disk (Linux file) file  descriptor.  It is  a simple to implement function. If success, 0 will be returned, if error, -1 will be returned.




int <strong>vsfs_create</strong> (char *filename).

With this, an applicaton will create a new file with name  filename. Your library implementation of this function will use an entry in the root directory to store information about the created file, like its name, size, first data block number, etc. If success, 0 will be returned. If error, -1 will be returned.




int <strong>vsfs_open</strong> (char *filename, int mode).

With  this function an application will open a file. The name of the file to open is filename. The mode paramater specifies if the  file will be opened in readonly mode or in append-only mode. We can either read the file or append to it. A file can not be opened for both reading and appending at the same time. In your library you should have a open file table, and an entry in that table will be used for the opened file. The index of that entry can be returned as the return value of this function. Hence the return value will be a non-negative integer acting as a file descriptor to be used in subsequent file operations. If error, -1 will be returned.




int <strong>vsfs_getsize </strong>(int fd).

With this an application learns the size of a file whose descriptor is fd. File must be opened first. Returns the number of data bytes in the file. A file with no data in it (no content) has size 0. If error, returns -1.




int <strong>vsfs_close</strong> (int fd).

With this function an application will close a file whose descriptor is fd. The related open file table entry should be marked as free.




int <strong>vsfs_read</strong> (int fd, void *buf, int n).

With this, an application can read data from a file. fd is the file descriptor. buf is pointing to a memory area for which  space is allocated earlier with malloc (or it can be a static array). n is the amount of data to read. Upon failure, -1 will be returned. Otherwise, number of bytes sucessfully read will be returned.




int <strong>vsfs_append</strong> (int fd, void *buf, int n).

With this, an application can append new data to the file. The  parameter fd is the file descriptor. The parameter buf is pointing to (i.e.,  is the address of) a static array holding the data or a dynamically allocated memory space holding the data. The parameter n is the size of the data to write (append) into the file. If error, -1 will be returned. Otherwise, the number of bytes successfully appended will be returned.




int <strong>vsfs_delete</strong> (char *filename).

With this, an application can delete a file. The name of the file to be deleted is filename. If succesful, 0 will be returned. In case of an error, -1 will be returned. Assume, a process can open at most 16 files simultaneously. Hence your library should have an open file table with 16 entries.













<h1>1.2 File System Specification</h1>




The vsfs file system will have just a single directory, i.e., root directory, so that it will be simple to implement. No subdirectories are supported. The block size is 4 KB. Block 0 (first block) will contain superblock information.




The next 7 blocks, ie., blocks 1, 2, 3, 4, 5, 6, 7,  will contain the <em>root directory</em>. Fixed sized <em>directory entries</em> will be used. Directory entry size is 256 bytes. That means each disk block can hold 16 directory entries. In this way we can have at most 7×16 = 112 directory entries, hence the file system can store at most 112 files in the disk. Maximum filename is 64 characters long, incuding the null character at the end. A directory entry for a file will contain filename and some other attributes that you find necessary.




FAT (<em>file allocation table</em>)  method is used to keep track of blocks allocated to files and free blocks. <em>FAT entry size</em> is 8 bytes. FAT will be stored after the root directory in the disk. FAT size will be constant and FAT will occupy 256 blocks. Hence, after the root directory blocks,  the next 256 disk blocks will store the FAT informaton. FAT will occupy 256 disk blocks no matter how big the virtual disk is. Each disk block can store 4096/8 = 512 FAT entries. Hence, FAT will have 256 x 512 = 128K entries. With those many entries possible in the FAT, the maximum disk size can be 128K x 4 KB = <strong>512 MB</strong>. After FAT, data blocks will come in the virtual disk.  The rest of the disk will be data blocks.  The disk should be large enough to hold at least the FAT, the root directory and the superblock.  Hence minimum disk size is 256+7+1 = 264 disk blocks, which is slightly more than <strong>1 MB. </strong>




The rest is upto you to specify. For example, you will decide what to keep in a directory entry, in a FAT entry, etc.




<h1>1.3 Experimentation and Report</h1>




Do some timing experiments and write a report about the results.  Try to measure how long it takes to create, read, write a file. Try various sizes. Plot results. Try to draw conclusions.




In your report, you must also write the details of your file system design; the structure of entries, etc.


