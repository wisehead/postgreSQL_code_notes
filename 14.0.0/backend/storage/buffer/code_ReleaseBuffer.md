#1.ReleaseBuffer

```
ReleaseBuffer
--if (BufferIsLocal(buffer))
----ResourceOwnerForgetBuffer
------ResourceArrayRemove
----LocalRefCount[-buffer - 1]--;
----return
--UnpinBuffer//todo
```