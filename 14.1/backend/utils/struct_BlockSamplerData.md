#1.struct BlockSamplerData

```cpp
/* Data structure for Algorithm S from Knuth 3.4.2 */
typedef struct
{
	BlockNumber N;				/* number of blocks, known in advance */
	int			n;				/* desired sample size */
	BlockNumber t;				/* current block number */
	int			m;				/* blocks selected so far */
	SamplerRandomState randstate;	/* random generator state */
} BlockSamplerData;
```