#1.struct LOCKTAG

```cpp

/*
 * The LOCKTAG struct is defined with malice aforethought to fit into 16
 * bytes with no padding.  Note that this would need adjustment if we were
 * to widen Oid, BlockNumber, or TransactionId to more than 32 bits.
 *
 * We include lockmethodid in the locktag so that a single hash table in
 * shared memory can store locks of different lockmethods.
 */
typedef struct LOCKTAG
{
    uint32      locktag_field1; /* a 32-bit ID field */
    uint32      locktag_field2; /* a 32-bit ID field */
    uint32      locktag_field3; /* a 32-bit ID field */
    uint16      locktag_field4; /* a 16-bit ID field */
    uint8       locktag_type;   /* see enum LockTagType */
    uint8       locktag_lockmethodid;   /* lockmethod indicator */
} LOCKTAG
```