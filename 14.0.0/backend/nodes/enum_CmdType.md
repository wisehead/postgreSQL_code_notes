#1.enum CmdType

```cpp
/*
 * CmdType -
 *	  enums for type of operation represented by a Query or PlannedStmt
 *
 * This is needed in both parsenodes.h and plannodes.h, so put it here...
 */
typedef enum CmdType
{
	CMD_UNKNOWN,
	CMD_SELECT,					/* select stmt */
	CMD_UPDATE,					/* update stmt */
	CMD_INSERT,					/* insert stmt */
	CMD_DELETE,
	CMD_UTILITY,				/* cmds like create, destroy, copy, vacuum,
								 * etc. */
	CMD_NOTHING					/* dummy command for instead nothing rules
								 * with qual */
} CmdType;

```