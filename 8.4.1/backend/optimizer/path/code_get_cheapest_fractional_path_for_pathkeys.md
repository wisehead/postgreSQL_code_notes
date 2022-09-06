#1.get_cheapest_fractional_path_for_pathkeys

```cpp
/*
 * get_cheapest_fractional_path_for_pathkeys
 *    Find the cheapest path (for retrieving a specified fraction of all
 *    the tuples) that satisfies the given pathkeys.
 *    Return NULL if no such path.
 *
 * See compare_fractional_path_costs() for the interpretation of the fraction
 * parameter.
 *
 * 'paths' is a list of possible paths that all generate the same relation
 * 'pathkeys' represents a required ordering (already canonicalized!)
 * 'fraction' is the fraction of the total tuples expected to be retrieved
 */
Path *
get_cheapest_fractional_path_for_pathkeys(List *paths,
                                          List *pathkeys,
                                          double fraction)
```

#2.caller

- query_planner