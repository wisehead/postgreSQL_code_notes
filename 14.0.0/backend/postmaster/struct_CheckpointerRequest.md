#1.struct CheckpointerRequest

```cpp
typedef struct
{
	SyncRequestType type;		/* request type */
	FileTag		ftag;			/* file identifier */
} CheckpointerRequest;
```