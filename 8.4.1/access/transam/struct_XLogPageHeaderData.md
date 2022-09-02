#1.struct XLogPageHeaderData

```cpp
typedef struct XLogPageHeaderData
{
    uint16      xlp_magic;      /* magic value for correctness checks */
    uint16      xlp_info;       /* flag bits, see below */
    TimeLineID  xlp_tli;        /* TimeLineID of first record on page */
    XLogRecPtr  xlp_pageaddr;   /* XLOG address of this page */
} XLogPageHeaderData;

#define SizeOfXLogShortPHD  MAXALIGN(sizeof(XLogPageHeaderData))

typedef XLogPageHeaderData *XLogPageHeader;

```