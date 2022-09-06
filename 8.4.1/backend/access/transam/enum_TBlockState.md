#1.enum TBlockState

```cpp
/*
 *  transaction block states - transaction state of client queries
 *
 * Note: the subtransaction states are used only for non-topmost
 * transactions; the others appear only in the topmost transaction.
 */
typedef enum TBlockState
{
    /* not-in-transaction-block states */
    TBLOCK_DEFAULT,             /* idle */
    TBLOCK_STARTED,             /* running single-query transaction */

    /* transaction block states */
    TBLOCK_BEGIN,               /* starting transaction block */
    TBLOCK_INPROGRESS,          /* live transaction */
    TBLOCK_END,                 /* COMMIT received */
    TBLOCK_ABORT,               /* failed xact, awaiting ROLLBACK */
    TBLOCK_ABORT_END,           /* failed xact, ROLLBACK received */
    TBLOCK_ABORT_PENDING,       /* live xact, ROLLBACK received */
    TBLOCK_PREPARE,             /* live xact, PREPARE received */

    /* subtransaction states */
    TBLOCK_SUBBEGIN,            /* starting a subtransaction */
    TBLOCK_SUBINPROGRESS,       /* live subtransaction */
    TBLOCK_SUBEND,              /* RELEASE received */
    TBLOCK_SUBABORT,            /* failed subxact, awaiting ROLLBACK */
    TBLOCK_SUBABORT_END,        /* failed subxact, ROLLBACK received */
    TBLOCK_SUBABORT_PENDING,    /* live subxact, ROLLBACK received */
    TBLOCK_SUBRESTART,          /* live subxact, ROLLBACK TO received */
    TBLOCK_SUBABORT_RESTART     /* failed subxact, ROLLBACK TO received */
} TBlockState;
```