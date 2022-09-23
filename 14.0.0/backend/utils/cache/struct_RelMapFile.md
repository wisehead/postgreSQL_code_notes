#1.struc RelMapFile

```cpp
typedef struct RelMapFile
{
	int32		magic;			/* always RELMAPPER_FILEMAGIC */
	int32		num_mappings;	/* number of valid RelMapping entries */
	RelMapping	mappings[MAX_MAPPINGS];
	pg_crc32c	crc;			/* CRC of all above */
	int32		pad;			/* to make the struct size be 512 exactly */
} RelMapFile;
```