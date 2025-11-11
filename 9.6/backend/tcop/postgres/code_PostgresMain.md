#1.PostgresMain

```cpp
PostgresMain(int argc, char *argv[], const char *dbname, const char *username)
{...
    if (sigsetjmp(local_sigjmp_buf, 1) != 0)  //返回非0，表示由siglongjmp()跳转回来，
即从“PG_RE_THROW()”执行后返回到这里
    {...
        /* Make sure libpq is in a good state */
        pq_comm_reset();   //错误发生后，执行一些恢复环境的工作
        EmitErrorReport();  /* Report the error to the client and/or server log */
...
        AbortCurrentTransaction();
//回滚当前报告错误的事务。此处是隐式回滚事务的重要代码，当捕获错误，则回滚事务
...
    }
...}


```