#1.UnlockBufHdr

```cpp
#define UnlockBufHdr(desc, s)	\
	do {	\
		pg_write_barrier(); \
		pg_atomic_write_u32(&(desc)->state, (s) & (~BM_LOCKED)); \
	} while (0)
```