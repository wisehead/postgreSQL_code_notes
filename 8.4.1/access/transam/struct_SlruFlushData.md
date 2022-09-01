#1.struct SlruFlushData

```cpp
typedef struct SlruFlushData
{
    int         num_files;      /* # files actually open */
    int         fd[MAX_FLUSH_BUFFERS];  /* their FD's */
    int         segno[MAX_FLUSH_BUFFERS];       /* their log seg#s */
} SlruFlushData;

```