#1.struct RelFileNodeBackend

```cpp
/*
 * Augmenting a relfilenode with the backend ID provides all the information
 * we need to locate the physical storage.  The backend ID is InvalidBackendId
 * for regular relations (those accessible to more than one backend), or the
 * owning backend's ID for backend-local relations.  Backend-local relations
 * are always transient and removed in case of a database crash; they are
 * never WAL-logged or fsync'd.
 */
typedef struct RelFileNodeBackend
{
	RelFileNode node;
	BackendId	backend;
} RelFileNodeBackend;
```