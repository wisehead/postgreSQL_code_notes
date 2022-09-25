#1.enum ScanOptions

```cpp
/*
 * Bitmask values for the flags argument to the scan_begin callback.
 */
typedef enum ScanOptions
{
	/* one of SO_TYPE_* may be specified */
	SO_TYPE_SEQSCAN = 1 << 0,
	SO_TYPE_BITMAPSCAN = 1 << 1,
	SO_TYPE_SAMPLESCAN = 1 << 2,
	SO_TYPE_TIDSCAN = 1 << 3,
	SO_TYPE_TIDRANGESCAN = 1 << 4,
	SO_TYPE_ANALYZE = 1 << 5,

	/* several of SO_ALLOW_* may be specified */
	/* allow or disallow use of access strategy */
	SO_ALLOW_STRAT = 1 << 6,
	/* report location to syncscan logic? */
	SO_ALLOW_SYNC = 1 << 7,
	/* verify visibility page-at-a-time? */
	SO_ALLOW_PAGEMODE = 1 << 8,

	/* unregister snapshot at scan end? */
	SO_TEMP_SNAPSHOT = 1 << 9
} ScanOptions;
```