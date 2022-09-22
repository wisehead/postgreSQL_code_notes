#1.InitLocalBuffers

```
InitLocalBuffers
--LocalBufferDescriptors = (BufferDesc *) calloc(nbufs, sizeof(BufferDesc));
--LocalBufferBlockPointers = (Block *) calloc(nbufs, sizeof(Block));
--LocalRefCount = (int32 *) calloc(nbufs, sizeof(int32));
--for (i = 0; i < nbufs; i++)
----BufferDesc *buf = GetLocalBufferDescriptor(i);
----buf->buf_id = -i - 2;
```