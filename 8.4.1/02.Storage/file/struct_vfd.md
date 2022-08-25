#1. struct vfd

```cpp
typedef struct vfd
{
    int         fd;             /* current FD, or VFD_CLOSED if none */
    unsigned short fdstate;     /* bitflags for VFD's state */
    ResourceOwner resowner;     /* owner, for automatic cleanup */
    File        nextFree;       /* link to next free VFD, if in freelist */
    File        lruMoreRecently;    /* doubly linked recency-of-use list */
    File        lruLessRecently;
    off_t       seekPos;        /* current logical file position */
    char       *fileName;       /* name of file, or NULL for unused VFD */
    /* NB: fileName is malloc'd, and must be free'd when closing the VFD */
    int         fileFlags;      /* open(2) flags for (re)opening the file */
    int         fileMode;       /* mode to pass to open(2) */
} Vfd;
```