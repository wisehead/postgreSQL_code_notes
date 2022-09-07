#1.struct SortCoordinateData

```cpp
/*
 * Tuplesort parallel coordination state, allocated by each participant in
 * local memory.  Participant caller initializes everything.  See usage notes
 * below.
 */
typedef struct SortCoordinateData
{
	/* Worker process?  If not, must be leader. */
	bool		isWorker;

	/*
	 * Leader-process-passed number of participants known launched (workers
	 * set this to -1).  Includes state within leader needed for it to
	 * participate as a worker, if any.
	 */
	int			nParticipants;

	/* Private opaque state (points to shared memory) */
	Sharedsort *sharedsort;
}			SortCoordinateData;

typedef struct SortCoordinateData *SortCoordinate;

```