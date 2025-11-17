#1.PortalStart

```cpp
/*
在上级函数调用exec_simple_query()，即同一个事务内部多次执行查询的时候，snapshot给定的参数值为InvalidSnapshot，所以通过调用GetTransactionSnapshot()和GetActiveSnapshot()获取的都是当前同一个快照。
*/

void   //exec_simple_query（）调用本函数时，传递参数“PortalStart(portal, NULL, 0, InvalidSnapshot);”，
PortalStart(Portal portal, ParamListInfo params, int eflags, Snapshot snapshot)
//即“snapshot == InvalidSnapshot”
{...
        switch (portal->strategy)
        {
            case PORTAL_ONE_SELECT:
                /* Must set snapshot before starting executor. */
                if (snapshot) // snapshot给定的参数值为InvalidSnapshot时，不走此分之
                    PushActiveSnapshot(snapshot);
                else  //可重复读级别，“GetTransactionSnapshot()”则一直返回的是
                    “FirstXactSnapshot”，所以快照使用的是同一个
                    PushActiveSnapshot(GetTransactionSnapshot());
                /* Create QueryDesc in portal's context; for the moment, set the
                destination to DestNone. */
                queryDesc = CreateQueryDesc((PlannedStmt *) linitial(portal-
                >stmts), portal->sourceText,
                                            GetActiveSnapshot(),
                                          InvalidSnapshot, None_Receiver, params, 0);
...
        }
...
}
```