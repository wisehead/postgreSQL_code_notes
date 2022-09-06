#1.ChoosePortalStrategy

```cpp
//执行策略选择器主函数
/*
 * ChoosePortalStrategy
 *      Select portal execution strategy given the intended statement list.
 *
 * The list elements can be Querys, PlannedStmts, or utility statements.
 * That's more general than portals need, but plancache.c uses this too.
 *
 * See the comments in portal.h.
 */
PortalStrategy
ChoosePortalStrategy(List *stmts)

```