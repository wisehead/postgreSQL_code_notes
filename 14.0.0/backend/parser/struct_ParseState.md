#1.struct ParseState

```cpp
/*
 * State information used during parse analysis
 *
 * parentParseState: NULL in a top-level ParseState.  When parsing a subquery,
 * links to current parse state of outer query.
 *
 * p_sourcetext: source string that generated the raw parsetree being
 * analyzed, or NULL if not available.  (The string is used only to
 * generate cursor positions in error messages: we need it to convert
 * byte-wise locations in parse structures to character-wise cursor
 * positions.)
 *
 * p_rtable: list of RTEs that will become the rangetable of the query.
 * Note that neither relname nor refname of these entries are necessarily
 * unique; searching the rtable by name is a bad idea.
 *
 * p_joinexprs: list of JoinExpr nodes associated with p_rtable entries.
 * This is one-for-one with p_rtable, but contains NULLs for non-join
 * RTEs, and may be shorter than p_rtable if the last RTE(s) aren't joins.
 *
 * p_joinlist: list of join items (RangeTblRef and JoinExpr nodes) that
 * will become the fromlist of the query's top-level FromExpr node.
 *
 * p_namespace: list of ParseNamespaceItems that represents the current
 * namespace for table and column lookup.  (The RTEs listed here may be just
 * a subset of the whole rtable.  See ParseNamespaceItem comments below.)
 *
 * p_lateral_active: true if we are currently parsing a LATERAL subexpression
 * of this parse level.  This makes p_lateral_only namespace items visible,
 * whereas they are not visible when p_lateral_active is FALSE.
 *
 * p_ctenamespace: list of CommonTableExprs (WITH items) that are visible
 * at the moment.  This is entirely different from p_namespace because a CTE
 * is not an RTE, rather "visibility" means you could make an RTE from it.
 *
 * p_future_ctes: list of CommonTableExprs (WITH items) that are not yet
 * visible due to scope rules.  This is used to help improve error messages.
 *
 * p_parent_cte: CommonTableExpr that immediately contains the current query,
 * if any.
 *
 * p_target_relation: target relation, if query is INSERT, UPDATE, or DELETE.
 *
 * p_target_nsitem: target relation's ParseNamespaceItem.
 *
 * p_is_insert: true to process assignment expressions like INSERT, false
 * to process them like UPDATE.  (Note this can change intra-statement, for
 * cases like INSERT ON CONFLICT UPDATE.)
 *
 * p_windowdefs: list of WindowDefs representing WINDOW and OVER clauses.
 * We collect these while transforming expressions and then transform them
 * afterwards (so that any resjunk tlist items needed for the sort/group
 * clauses end up at the end of the query tlist).  A WindowDef's location in
 * this list, counting from 1, is the winref number to use to reference it.
 *
 * p_expr_kind: kind of expression we're currently parsing, as per enum above;
 * EXPR_KIND_NONE when not in an expression.
 *
 * p_next_resno: next TargetEntry.resno to assign, starting from 1.
 *
 * p_multiassign_exprs: partially-processed MultiAssignRef source expressions.
 *
 * p_locking_clause: query's FOR UPDATE/FOR SHARE clause, if any.
 *
 * p_locked_from_parent: true if parent query level applies FOR UPDATE/SHARE
 * to this subquery as a whole.
 *
 * p_resolve_unknowns: resolve unknown-type SELECT output columns as type TEXT
 * (this is true by default).
 *
 * p_hasAggs, p_hasWindowFuncs, etc: true if we've found any of the indicated
 * constructs in the query.
 *
 * p_last_srf: the set-returning FuncExpr or OpExpr most recently found in
 * the query, or NULL if none.
 *
 * p_pre_columnref_hook, etc: optional parser hook functions for modifying the
 * interpretation of ColumnRefs and ParamRefs.
 *
 * p_ref_hook_state: passthrough state for the parser hook functions.
 */
struct ParseState
{
	ParseState *parentParseState;	/* stack link */
	const char *p_sourcetext;	/* source text, or NULL if not available */
	List	   *p_rtable;		/* range table so far */
	List	   *p_joinexprs;	/* JoinExprs for RTE_JOIN p_rtable entries */
	List	   *p_joinlist;		/* join items so far (will become FromExpr
								 * node's fromlist) */
	List	   *p_namespace;	/* currently-referenceable RTEs (List of
								 * ParseNamespaceItem) */
	bool		p_lateral_active;	/* p_lateral_only items visible? */
	List	   *p_ctenamespace; /* current namespace for common table exprs */
	List	   *p_future_ctes;	/* common table exprs not yet in namespace */
	CommonTableExpr *p_parent_cte;	/* this query's containing CTE */
	Relation	p_target_relation;	/* INSERT/UPDATE/DELETE target rel */
	ParseNamespaceItem *p_target_nsitem;	/* target rel's NSItem, or NULL */
	bool		p_is_insert;	/* process assignment like INSERT not UPDATE */
	List	   *p_windowdefs;	/* raw representations of window clauses */
	ParseExprKind p_expr_kind;	/* what kind of expression we're parsing */
	int			p_next_resno;	/* next targetlist resno to assign */
	List	   *p_multiassign_exprs;	/* junk tlist entries for multiassign */
	List	   *p_locking_clause;	/* raw FOR UPDATE/FOR SHARE info */
	bool		p_locked_from_parent;	/* parent has marked this subquery
										 * with FOR UPDATE/FOR SHARE */
	bool		p_resolve_unknowns; /* resolve unknown-type SELECT outputs as
									 * type text */

	QueryEnvironment *p_queryEnv;	/* curr env, incl refs to enclosing env */

	/* Flags telling about things found in the query: */
	bool		p_hasAggs;
	bool		p_hasWindowFuncs;
	bool		p_hasTargetSRFs;
	bool		p_hasSubLinks;
	bool		p_hasModifyingCTE;

	Node	   *p_last_srf;		/* most recent set-returning func/op found */

	/*
	 * Optional hook functions for parser callbacks.  These are null unless
	 * set up by the caller of make_parsestate.
	 */
	PreParseColumnRefHook p_pre_columnref_hook;
	PostParseColumnRefHook p_post_columnref_hook;
	ParseParamRefHook p_paramref_hook;
	CoerceParamHook p_coerce_param_hook;
	void	   *p_ref_hook_state;	/* common passthrough link for above */
};


```