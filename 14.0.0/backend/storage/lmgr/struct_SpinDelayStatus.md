#1.struct SpinDelayStatus

```cpp
/*
 * Support for spin delay which is useful in various places where
 * spinlock-like procedures take place.
 */
typedef struct
{
	int			spins;
	int			delays;
	int			cur_delay;
	const char *file;
	int			line;
	const char *func;
} SpinDelayStatus;
```