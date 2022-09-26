#1.CreateMetaPage

```
CreateMetaPage
--buf = IvfflatNewBuffer(index, forkNum);
----buf = ReadBufferExtended(index, forkNum, P_NEW, RBM_NORMAL, NULL);
------ReadBuffer_common
----LockBuffer(buf, BUFFER_LOCK_EXCLUSIVE);
--IvfflatInitRegisterPage
--IvfflatCommitBuffer
```

#2. IvfflatInitRegisterPage
```
IvfflatInitRegisterPage
--GenericXLogStart
--GenericXLogRegisterBuffer
--IvfflatInitPage
----PageInit
```

#3.IvfflatCommitBuffer

```
IvfflatCommitBuffer
--MarkBufferDirty
--GenericXLogFinish
```