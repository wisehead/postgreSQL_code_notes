#1.struct f_smgr

```cpp
/*
 * This struct of function pointers defines the API between smgr.c and
 * any individual storage manager module.  Note that smgr subfunctions are
 * generally expected to report problems via elog(ERROR).  An exception is
 * that smgr_unlink should use elog(WARNING), rather than erroring out,
 * because we normally unlink relations during post-commit/abort cleanup,
 * and so it's too late to raise an error.  Also, various conditions that
 * would normally be errors should be allowed during bootstrap and/or WAL
 * recovery --- see comments in md.c for details.
 */
typedef struct f_smgr
{
    void        (*smgr_init) (void);    /* may be NULL */
    void        (*smgr_shutdown) (void);    /* may be NULL */
    void        (*smgr_open) (SMgrRelation reln);
    void        (*smgr_close) (SMgrRelation reln, ForkNumber forknum);
    void        (*smgr_create) (SMgrRelation reln, ForkNumber forknum,
                                bool isRedo);
    bool        (*smgr_exists) (SMgrRelation reln, ForkNumber forknum);
    void        (*smgr_unlink) (RelFileNodeBackend rnode, ForkNumber forknum,
                                bool isRedo);
    void        (*smgr_extend) (SMgrRelation reln, ForkNumber forknum,
                                BlockNumber blocknum, char *buffer, bool skipFsync);
    bool        (*smgr_prefetch) (SMgrRelation reln, ForkNumber forknum,
                                  BlockNumber blocknum);
    void        (*smgr_read) (SMgrRelation reln, ForkNumber forknum,
                              BlockNumber blocknum, char *buffer);
    void        (*smgr_write) (SMgrRelation reln, ForkNumber forknum,
                               BlockNumber blocknum, char *buffer, bool skipFsync);
    void        (*smgr_writeback) (SMgrRelation reln, ForkNumber forknum,
                                   BlockNumber blocknum, BlockNumber nblocks);
    BlockNumber (*smgr_nblocks) (SMgrRelation reln, ForkNumber forknum);
    void        (*smgr_truncate) (SMgrRelation reln, ForkNumber forknum,
                                  BlockNumber nblocks);
    void        (*smgr_immedsync) (SMgrRelation reln, ForkNumber forknum);
} f_smgr;

```