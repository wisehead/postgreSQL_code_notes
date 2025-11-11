#1.PushTransaction

```cpp
保存点的实现方式，是通过支持定义保存点名称，然后在数据库引擎内部开启一个子事务块（PushTransaction()函数）​，之后进入事务有限状态自动机中调用StartSubTransaction()函数来初始化子事务，之后通过CommitSubTransaction()或AbortSubTransaction()来对子事务进行提交或回滚。

```