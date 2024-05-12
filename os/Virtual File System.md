1. Allows userspace to access file independent of the concrete file system by creating abstractions
2. common interface
3. read / write/ open on various file system like ext4, fat32 / usb drive etc


Unix filesystems
- hirerachical tree of files
- filesystem can mean filesystem type or partition

File: string of bytes from file address 0 to address (file size-1)
- metadata: name, access permissions, modification date, etc
- separated from the file data into specific objects inodes, dentries
	- metadata stored in two objects (inodes  - everything else datablock location, who has access, etc) and dentries - (name related metadata, lookup related data)
- directory is a special kind of file
	- dentry cache - given path can we find the inode the given file - get disk block from particular inode

each process when it tries to access a file, it creates a file object, that is pointed to by the file descriptor

superblock:
1. each partition is a filesystem
2. contains information about the partition


Find inode -> get metadata -> get data block
Inode bitmap -> says which block is available when we want to allocate new inode


On disk we store **superblock, inode and the data** everything else maintained at runtime
