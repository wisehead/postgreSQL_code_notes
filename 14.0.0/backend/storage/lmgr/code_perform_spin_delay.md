#1.perform_spin_delay

```
perform_spin_delay
--SPIN_DELAY();
----spin_delay
--if (++(status->spins) >= spins_per_delay)
----if (++(status->delays) > NUM_DELAYS)
------s_lock_stuck(status->file, status->line, status->func);//print error
----if (status->cur_delay == 0) /* first time to delay? */
------status->cur_delay = MIN_DELAY_USEC;
----pg_usleep(status->cur_delay);
----status->cur_delay += (int) (status->cur_delay *
						((double) random() / (double) MAX_RANDOM_VALUE) + 0.5);
		/* wrap back to minimum delay when max is exceeded */
----if (status->cur_delay > MAX_DELAY_USEC)
------status->cur_delay = MIN_DELAY_USEC;
----status->spins = 0;
```