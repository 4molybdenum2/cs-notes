UFS -> add cylinder blocks (adopted from FFS) -> Ext 2 -> add journaling becomes Ext 3 -> more flexible mapping from inode to data blocks, maintain directory names in a more flexible format, larger files -> Ext 4


Cylinder group was introduced to minimize seek time (so that data blocks are in the same track)
Same cylinder group - data blocks in same track or in adjacent tracks
FFS introduced this

Ext2 adopted this (block group instead of cylinder group)
Directory access time is terrible in ext2


Ext3:
1. integrate journaling into ext2 - crash consistency
2. journaling = WAL in filesystems
3. journal is a circular array fixed size that is a special file with a hardcoded inode number
4. suppose program crashes while journaling (before tx end) when system comes back -> should do nothing because tx didn't end (all or nothing semantics), if crashes after tx ends (committed tx), redo it
5. after commit we can update data blocks on disk


undo logging? - another design


In ReiserFS journal is a dedicated section at the beginning of the FS

Journal recovery:
1. failure between commit and checkpointing:
	1. there is a tx end marker in the logs
	2. replay logs in last journal tx
2. failure before journal commit:
	1. ignore last journal tx




Journaling mode:
1. Journal:
	1. metadata and content are saved in journal
2. ordered:
	1. only metadata is saved in journal. metadata are journaled only after writing content to disk. this is **default**
3. writeback:
	1. only metadata is saved in journal. metadata is journaled either before or after writing to disk

- single append operation requires multiple disk accesses:
	- add data block (D1 -> D2)
	- update inode (I2 -> I2')
	- update datablock bitmap (B1 -> B2)



**Case without journaling:**

**Case 1a:**

Only B1 -> B2
I2, D1 old state
- only update bitmap and then system crash, after system recovers we see that data block is used in our bitmap. Inode says that no one is using it. Inconsistency

**Case 1b:**

Only I2 -> I2'
B1 and D1 old
- same as above


**Case 1c:**
D1 -> D2
old B1 and I2

No one is using D2 and no one is pointing, but still we have data in D2 -> reasonably ok?
We may lose some data whatever we try to access...but still ok...
- motivates journaling only metadata and not the data


**Case 1d:**
Inode points to garbage data block





**Case 2a:**

B2, D2 updated
I2 old

If metadata do not agree with each other -> fs inconsistent

**Case 2b:**
I2', D2 updated
B1 old

If metadata do not agree with each other -> fs inconsistent

**Case 2c:**
B2, I2'
D1 old

metadata agree that is indeed pointing to D2



- For performance improvement has a volatile cache - data blocks has its own cache -> first aggregates then writes to hard drive
- Flush request to ensure data content are persisted, flushing from their cache (sync)



**Ext4**:
- Indirect block map -> extent tree
- Htree replaces List


Paper: Analysis of Journaling File Systems


1. Semantic Block Analysis to compare various file systems
2. Semantic Trace Playback to compare slight changes in ext3



**Analysis of ext3**:

Figure 3:
1. 