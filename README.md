# Notes from eXpOS

Link to roadmap: https://exposnitc.github.io/expos-docs/roadmap/

# Terms

## System Development

- SPL, System Programming Language = an enriched XSM assembly language to write OS modules
- XSM, eXperimental String Machine = an architecture simulator
- ExpL, Experimental Language = a tiny high level programming language to write apps executable by eXpOS
- eXpOS, Experimental OS = the kernel for XSM architecture
- eXpFS, Experimental File System = the OS file system
- eXpOS Library = provides a generic interface through which other applications can access the OS system calls
- XFS interface = tool to transfer executable kernel modules from your host (Linux/Unix) system to specified blocks of the XSM disk

## Disk

- XSM Disk contains 512 blocks
  - each block contains 512 words
  - each word is 16 characters long
    - conventional ascii character is 8 bit. My guess, 1 word = 16 bytes = 128 bits
- Disk organization
  - block 0-1 = Bootstrap
  - block 2 = Disk Free List `df`, to see which block is occupied
  - block 3-4 (total 1024 words)
    - first 960 words are inode table (with 60 entries. Each entry is 16 words)
    - next 32 words are user table (with 16 entries. Each entry is 2 words)
    - last 32 words are reserved for future use.
  - block 5 = root file (first 480 words. 32 words unallocated)
  - blocks 0-68 (total 69 blocks)
    - stores various OS data structures
    - stores routines, idle code
    - stores INIT program
  - blocks 69-511 are free to use

- inode table, index node table = data structure in UNIX-like OS that contains important information pertaining to files within a file system
  - a copy of the table is also maintained in the memory when OS is running
  - it can only contain MAX_FILE_NUM (= 60) of entries
  - each entry contains: file name, size, type, data block numbers of a file stored in the disk, userid, user permission
  - each file is limited by MAX_FILE_SIZE (= 2048 words = 4 blocks)
  - the position of inode entry corresponds to position of file in disk
    - and whether the file occupy all 4 reserved blocks are indicated by 4 fields in the inode entry

# Set Up

```bash
docker build -t expos:ubuntu20.04 .

docker run -v $PWD/workdir:/home/expos/myexpos/workdir -d --name expos -i expos:ubuntu20.04

docker start expos # if the container instance is not already running
docker stop expos

docker exec -it expos /bin/bash # to get a bash shell inside the container
```