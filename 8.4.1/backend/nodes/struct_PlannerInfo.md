#1.struct PlannerInfo

```cpp
/*----------
 * PlannerInfo
 *      Per-query information for planning/optimization
 *
 * This struct is conventionally called "root" in all the planner routines.
 * It holds links to all of the planner's working state, in addition to the
 * original Query.  Note that at present the planner extensively modifies
 * the passed-in Query data structure; someday that should stop.
 *----------
 */
 
typedef struct PlannerInfo
{
    NodeTag     type;

    Query      *parse;          /* the Query being planned */

    PlannerGlobal *glob;        /* global info for current planner run */

    Index       query_level;    /* 1 at the outermost Query */

    struct PlannerInfo *parent_root;    /* NULL at outermost Query */

    /*
     * simple_rel_array holds pointers to "base rels" and "other rels" (see
     * comments for RelOptInfo for more info).  It is indexed by rangetable
     * index (so entry 0 is always wasted).  Entries can be NULL when an RTE
     * does not correspond to a base relation, such as a join RTE or an
     * unreferenced view RTE; or if the RelOptInfo hasn't been made yet.
     */
    struct RelOptInfo **simple_rel_array;       /* All 1-rel RelOptInfos */
    int         simple_rel_array_size;  /* allocated size of array */

    /*
     * simple_rte_array is the same length as simple_rel_array and holds
     * pointers to the associated rangetable entries.  This lets us avoid
     * rt_fetch(), which can be a bit slow once large inheritance sets have
     * been expanded.
     */
    RangeTblEntry **simple_rte_array;   /* rangetable as an array */

    /*
     * join_rel_list is a list of all join-relation RelOptInfos we have
     * considered in this planning run.  For small problems we just scan the
     * list to do lookups, but when there are many join relations we build a
     * hash table for faster lookups.  The hash table is present and valid
     * when join_rel_hash is not NULL.  Note that we still maintain the list
     * even when using the hash table for lookups; this simplifies life for
     * GEQO.
     */
    List       *join_rel_list;  /* list of join-relation RelOptInfos */
    struct HTAB *join_rel_hash; /* optional hashtable for join relations */

    List       *resultRelations;    /* integer list of RT indexes, or NIL */

    List       *returningLists; /* list of lists of TargetEntry, or NIL */

    List       *init_plans;     /* init SubPlans for query */

    List       *cte_plan_ids;   /* per-CTE-item list of subplan IDs */

    List       *eq_classes;     /* list of active EquivalenceClasses */

    List       *canon_pathkeys; /* list of "canonical" PathKeys */

    List       *left_join_clauses;      /* list of RestrictInfos for
                                         * mergejoinable outer join clauses
                                         * w/nonnullable var on left */

    List       *right_join_clauses;     /* list of RestrictInfos for
                                         * mergejoinable outer join clauses
                                         * w/nonnullable var on right */

    List       *full_join_clauses;      /* list of RestrictInfos for
                                         * mergejoinable full join clauses */

    List       *join_info_list; /* list of SpecialJoinInfos */

    List       *append_rel_list;    /* list of AppendRelInfos */

    List       *placeholder_list;       /* list of PlaceHolderInfos */

    List       *query_pathkeys; /* desired pathkeys for query_planner(), and
                                 * actual pathkeys afterwards */

    List       *group_pathkeys; /* groupClause pathkeys, if any */
    List       *window_pathkeys;    /* pathkeys of bottom window, if any */
    List       *distinct_pathkeys;      /* distinctClause pathkeys, if any */
    List       *sort_pathkeys;  /* sortClause pathkeys, if any */

    List       *initial_rels;   /* RelOptInfos we are now trying to join */

    MemoryContext planner_cxt;  /* context holding PlannerInfo */

    double      total_table_pages;      /* # of pages in all tables of query */

    double      tuple_fraction; /* tuple_fraction passed to query_planner */

    bool        hasJoinRTEs;    /* true if any RTEs are RTE_JOIN kind */
    bool        hasHavingQual;  /* true if havingQual was non-null */
    bool        hasPseudoConstantQuals; /* true if any RestrictInfo has
                                         * pseudoconstant = true */
    bool        hasRecursion;   /* true if planning a recursive WITH item */

    /* These fields are used only when hasRecursion is true: */
    int         wt_param_id;    /* PARAM_EXEC ID for the work table */
    struct Plan *non_recursive_plan;    /* plan for non-recursive term */

    /* Added at end to minimize ABI breakage in 8.4 branch: */

    bool        hasInheritedTarget; /* true if parse->resultRelation is an
                                     * inheritance child rel */

    /* Added post-release, will be in a saner place in 9.3: */
    List       *plan_params;    /* list of PlannerParamItems, see below */
} PlannerInfo;
```