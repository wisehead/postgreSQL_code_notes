#1.union ExprEvalStep

```cpp
typedef struct ExprEvalStep
{
	/*
	 * Instruction to be executed.  During instruction preparation this is an
	 * enum ExprEvalOp, but later it can be changed to some other type, e.g. a
	 * pointer for computed goto (that's why it's an intptr_t).
	 */
	intptr_t	opcode;

	/* where to store the result of this step */
	Datum	   *resvalue;
	bool	   *resnull;

	/*
	 * Inline data for the operation.  Inline data is faster to access, but
	 * also bloats the size of all instructions.  The union should be kept to
	 * no more than 40 bytes on 64-bit systems (so that the entire struct is
	 * no more than 64 bytes, a single cacheline on common systems).
	 */
	union
	{
		/* for EEOP_INNER/OUTER/SCAN_FETCHSOME */
		struct
		{
			/* attribute number up to which to fetch (inclusive) */
			int			last_var;
			/* will the type of slot be the same for every invocation */
			bool		fixed;
			/* tuple descriptor, if known */
			TupleDesc	known_desc;
			/* type of slot, can only be relied upon if fixed is set */
			const TupleTableSlotOps *kind;
		}			fetch;

		/* for EEOP_INNER/OUTER/SCAN_[SYS]VAR[_FIRST] */
		struct
		{
			/* attnum is attr number - 1 for regular VAR ... */
			/* but it's just the normal (negative) attr number for SYSVAR */
			int			attnum;
			Oid			vartype;	/* type OID of variable */
		}			var;

		/* for EEOP_WHOLEROW */
		struct
		{
			Var		   *var;	/* original Var node in plan tree */
			bool		first;	/* first time through, need to initialize? */
			bool		slow;	/* need runtime check for nulls? */
			TupleDesc	tupdesc;	/* descriptor for resulting tuples */
			JunkFilter *junkFilter; /* JunkFilter to remove resjunk cols */
		}			wholerow;

		/* for EEOP_ASSIGN_*_VAR */
		struct
		{
			/* target index in ExprState->resultslot->tts_values/nulls */
			int			resultnum;
			/* source attribute number - 1 */
			int			attnum;
		}			assign_var;

		/* for EEOP_ASSIGN_TMP[_MAKE_RO] */
		struct
		{
			/* target index in ExprState->resultslot->tts_values/nulls */
			int			resultnum;
		}			assign_tmp;

		/* for EEOP_CONST */
		struct
		{
			/* constant's value */
			Datum		value;
			bool		isnull;
		}			constval;

		/* for EEOP_FUNCEXPR_* / NULLIF / DISTINCT */
		struct
		{
			FmgrInfo   *finfo;	/* function's lookup data */
			FunctionCallInfo fcinfo_data;	/* arguments etc */
			/* faster to access without additional indirection: */
			PGFunction	fn_addr;	/* actual call address */
			int			nargs;	/* number of arguments */
		}			func;

		/* for EEOP_BOOL_*_STEP */
		struct
		{
			bool	   *anynull;	/* track if any input was NULL */
			int			jumpdone;	/* jump here if result determined */
		}			boolexpr;

		/* for EEOP_QUAL */
		struct
		{
			int			jumpdone;	/* jump here on false or null */
		}			qualexpr;

		/* for EEOP_JUMP[_CONDITION] */
		struct
		{
			int			jumpdone;	/* target instruction's index */
		}			jump;

		/* for EEOP_NULLTEST_ROWIS[NOT]NULL */
		struct
		{
			/* cached descriptor for composite type - filled at runtime */
			ExprEvalRowtypeCache rowcache;
		}			nulltest_row;

		/* for EEOP_PARAM_EXEC/EXTERN */
		struct
		{
			int			paramid;	/* numeric ID for parameter */
			Oid			paramtype;	/* OID of parameter's datatype */
		}			param;

		/* for EEOP_PARAM_CALLBACK */
		struct
		{
			ExecEvalSubroutine paramfunc;	/* add-on evaluation subroutine */
			void	   *paramarg;	/* private data for same */
			int			paramid;	/* numeric ID for parameter */
			Oid			paramtype;	/* OID of parameter's datatype */
		}			cparam;

		/* for EEOP_CASE_TESTVAL/DOMAIN_TESTVAL */
		struct
		{
			Datum	   *value;	/* value to return */
			bool	   *isnull;
		}			casetest;

		/* for EEOP_MAKE_READONLY */
		struct
		{
			Datum	   *value;	/* value to coerce to read-only */
			bool	   *isnull;
		}			make_readonly;

		/* for EEOP_IOCOERCE */
		struct
		{
			/* lookup and call info for source type's output function */
			FmgrInfo   *finfo_out;
			FunctionCallInfo fcinfo_data_out;
			/* lookup and call info for result type's input function */
			FmgrInfo   *finfo_in;
			FunctionCallInfo fcinfo_data_in;
		}			iocoerce;

		/* for EEOP_SQLVALUEFUNCTION */
		struct
		{
			SQLValueFunction *svf;
		}			sqlvaluefunction;

		/* for EEOP_NEXTVALUEEXPR */
		struct
		{
			Oid			seqid;
			Oid			seqtypid;
		}			nextvalueexpr;

		/* for EEOP_ARRAYEXPR */
		struct
		{
			Datum	   *elemvalues; /* element values get stored here */
			bool	   *elemnulls;
			int			nelems; /* length of the above arrays */
			Oid			elemtype;	/* array element type */
			int16		elemlength; /* typlen of the array element type */
			bool		elembyval;	/* is the element type pass-by-value? */
			char		elemalign;	/* typalign of the element type */
			bool		multidims;	/* is array expression multi-D? */
		}			arrayexpr;

		/* for EEOP_ARRAYCOERCE */
		struct
		{
			ExprState  *elemexprstate;	/* null if no per-element work */
			Oid			resultelemtype; /* element type of result array */
			struct ArrayMapState *amstate;	/* workspace for array_map */
		}			arraycoerce;

		/* for EEOP_ROW */
		struct
		{
			TupleDesc	tupdesc;	/* descriptor for result tuples */
			/* workspace for the values constituting the row: */
			Datum	   *elemvalues;
			bool	   *elemnulls;
		}			row;

		/* for EEOP_ROWCOMPARE_STEP */
		struct
		{
			/* lookup and call data for column comparison function */
			FmgrInfo   *finfo;
			FunctionCallInfo fcinfo_data;
			PGFunction	fn_addr;
			/* target for comparison resulting in NULL */
			int			jumpnull;
			/* target for comparison yielding inequality */
			int			jumpdone;
		}			rowcompare_step;

		/* for EEOP_ROWCOMPARE_FINAL */
		struct
		{
			RowCompareType rctype;
		}			rowcompare_final;

		/* for EEOP_MINMAX */
		struct
		{
			/* workspace for argument values */
			Datum	   *values;
			bool	   *nulls;
			int			nelems;
			/* is it GREATEST or LEAST? */
			MinMaxOp	op;
			/* lookup and call data for comparison function */
			FmgrInfo   *finfo;
			FunctionCallInfo fcinfo_data;
		}			minmax;

		/* for EEOP_FIELDSELECT */
		struct
		{
			AttrNumber	fieldnum;	/* field number to extract */
			Oid			resulttype; /* field's type */
			/* cached descriptor for composite type - filled at runtime */
			ExprEvalRowtypeCache rowcache;
		}			fieldselect;

		/* for EEOP_FIELDSTORE_DEFORM / FIELDSTORE_FORM */
		struct
		{
			/* original expression node */
			FieldStore *fstore;

			/* cached descriptor for composite type - filled at runtime */
			/* note that a DEFORM and FORM pair share the same cache */
			ExprEvalRowtypeCache *rowcache;

			/* workspace for column values */
			Datum	   *values;
			bool	   *nulls;
			int			ncolumns;
		}			fieldstore;

		/* for EEOP_SBSREF_SUBSCRIPTS */
		struct
		{
			ExecEvalBoolSubroutine subscriptfunc;	/* evaluation subroutine */
			/* too big to have inline */
			struct SubscriptingRefState *state;
			int			jumpdone;	/* jump here on null */
		}			sbsref_subscript;

		/* for EEOP_SBSREF_OLD / ASSIGN / FETCH */
		struct
		{
			ExecEvalSubroutine subscriptfunc;	/* evaluation subroutine */
			/* too big to have inline */
			struct SubscriptingRefState *state;
		}			sbsref;

		/* for EEOP_DOMAIN_NOTNULL / DOMAIN_CHECK */
		struct
		{
			/* name of constraint */
			char	   *constraintname;
			/* where the result of a CHECK constraint will be stored */
			Datum	   *checkvalue;
			bool	   *checknull;
			/* OID of domain type */
			Oid			resulttype;
		}			domaincheck;

		/* for EEOP_CONVERT_ROWTYPE */
		struct
		{
			Oid			inputtype;	/* input composite type */
			Oid			outputtype; /* output composite type */
			/* these three fields are filled at runtime: */
			ExprEvalRowtypeCache *incache;	/* cache for input type */
			ExprEvalRowtypeCache *outcache; /* cache for output type */
			TupleConversionMap *map;	/* column mapping */
		}			convert_rowtype;

		/* for EEOP_SCALARARRAYOP */
		struct
		{
			/* element_type/typlen/typbyval/typalign are filled at runtime */
			Oid			element_type;	/* InvalidOid if not yet filled */
			bool		useOr;	/* use OR or AND semantics? */
			int16		typlen; /* array element type storage info */
			bool		typbyval;
			char		typalign;
			FmgrInfo   *finfo;	/* function's lookup data */
			FunctionCallInfo fcinfo_data;	/* arguments etc */
			/* faster to access without additional indirection: */
			PGFunction	fn_addr;	/* actual call address */
		}			scalararrayop;

		/* for EEOP_HASHED_SCALARARRAYOP */
		struct
		{
			bool		has_nulls;
			struct ScalarArrayOpExprHashTable *elements_tab;
			FmgrInfo   *finfo;	/* function's lookup data */
			FunctionCallInfo fcinfo_data;	/* arguments etc */
			/* faster to access without additional indirection: */
			PGFunction	fn_addr;	/* actual call address */
			FmgrInfo   *hash_finfo; /* function's lookup data */
			FunctionCallInfo hash_fcinfo_data;	/* arguments etc */
			/* faster to access without additional indirection: */
			PGFunction	hash_fn_addr;	/* actual call address */
		}			hashedscalararrayop;

		/* for EEOP_XMLEXPR */
		struct
		{
			XmlExpr    *xexpr;	/* original expression node */
			/* workspace for evaluating named args, if any */
			Datum	   *named_argvalue;
			bool	   *named_argnull;
			/* workspace for evaluating unnamed args, if any */
			Datum	   *argvalue;
			bool	   *argnull;
		}			xmlexpr;

		/* for EEOP_AGGREF */
		struct
		{
			int			aggno;
		}			aggref;

		/* for EEOP_GROUPING_FUNC */
		struct
		{
			List	   *clauses;	/* integer list of column numbers */
		}			grouping_func;

		/* for EEOP_WINDOW_FUNC */
		struct
		{
			/* out-of-line state, modified by nodeWindowAgg.c */
			WindowFuncExprState *wfstate;
		}			window_func;

		/* for EEOP_SUBPLAN */
		struct
		{
			/* out-of-line state, created by nodeSubplan.c */
			SubPlanState *sstate;
		}			subplan;

		/* for EEOP_AGG_*DESERIALIZE */
		struct
		{
			FunctionCallInfo fcinfo_data;
			int			jumpnull;
		}			agg_deserialize;

		/* for EEOP_AGG_STRICT_INPUT_CHECK_NULLS / STRICT_INPUT_CHECK_ARGS */
		struct
		{
			/*
			 * For EEOP_AGG_STRICT_INPUT_CHECK_ARGS args contains pointers to
			 * the NullableDatums that need to be checked for NULLs.
			 *
			 * For EEOP_AGG_STRICT_INPUT_CHECK_NULLS nulls contains pointers
			 * to booleans that need to be checked for NULLs.
			 *
			 * Both cases currently need to exist because sometimes the
			 * to-be-checked nulls are in TupleTableSlot.isnull array, and
			 * sometimes in FunctionCallInfoBaseData.args[i].isnull.
			 */
			NullableDatum *args;
			bool	   *nulls;
			int			nargs;
			int			jumpnull;
		}			agg_strict_input_check;

		/* for EEOP_AGG_PLAIN_PERGROUP_NULLCHECK */
		struct
		{
			int			setoff;
			int			jumpnull;
		}			agg_plain_pergroup_nullcheck;

		/* for EEOP_AGG_PLAIN_TRANS_[INIT_][STRICT_]{BYVAL,BYREF} */
		/* for EEOP_AGG_ORDERED_TRANS_{DATUM,TUPLE} */
		struct
		{
			AggStatePerTrans pertrans;
			ExprContext *aggcontext;
			int			setno;
			int			transno;
			int			setoff;
		}			agg_trans;
	}			d;
} ExprEvalStep;

```