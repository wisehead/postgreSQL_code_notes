#1.PGSharedMemoryCreate

```
PGSharedMemoryCreate
--stat(DataDir, &statbuf)
--NextShmemSegID = statbuf.st_ino;
--for (;;)
----memAddress = InternalIpcMemoryCreate(NextShmemSegID, sysvsize);
----//成功则返回
----shmid = shmget(NextShmemSegID, sizeof(PGShmemHeader), 0);//只分配管理头
----state = PGSharedMemoryAttach(shmid, NULL, &oldhdr);
----switch (state)
------case SHMSTATE_ANALYSIS_FAILURE:
------case SHMSTATE_ATTACHED:
--------errmsg
------case SHMSTATE_ENOENT:
--------errmsg
------case SHMSTATE_FOREIGN:
				NextShmemSegID++;
				break;
------case SHMSTATE_UNATTACHED:
--------shmctl(shmid, IPC_RMID, NULL)
----shmdt(oldhdr)
--//end for
--/* Initialize new segment. */
	hdr = (PGShmemHeader *) memAddress;
	hdr->creatorPID = getpid();
	hdr->magic = PGShmemMagic;
	hdr->dsm_control = 0;

	/* Fill in the data directory ID info, too */
	hdr->device = statbuf.st_dev;
	hdr->inode = statbuf.st_ino;

	/*
	 * Initialize space allocation status for segment.
	 */
	hdr->totalsize = size;
	hdr->freeoffset = MAXALIGN(sizeof(PGShmemHeader));
	*shim = hdr;

	/* Save info for possible future use */
	UsedShmemSegAddr = memAddress;
	UsedShmemSegID = (unsigned long) NextShmemSegID;
```