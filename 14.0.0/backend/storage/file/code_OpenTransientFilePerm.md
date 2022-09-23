#1.OpenTransientFilePerm

```
OpenTransientFilePerm
--reserveAllocatedDesc
--ReleaseLruFiles
--fd = BasicOpenFilePerm(fileName, fileFlags, fileMode);
--AllocateDesc *desc = &allocatedDescs[numAllocatedDescs];                                     
--desc->kind = AllocateDescRawF	 D;                        
--desc->desc.fd = fd;          	                           
--desc->create_subid = GetCurre	 ntSubTransactionId();     
--numAllocatedDescs++;         	                                                                                 
--return fd;                   	                           

```

#2.reserveAllocatedDesc

```
reserveAllocatedDesc
--if (numAllocatedDescs < maxAllocatedDescs)
----return true;
--if (allocatedDescs == NULL)
----newMax = FD_MINFREE / 3;
----newDescs = (AllocateDesc *) malloc(newMax * sizeof(AllocateDesc));
----allocatedDescs = newDescs;
----maxAllocatedDescs = newMax;
----return true;
--newMax = max_safe_fds / 3;
--if (newMax > maxAllocatedDescs)
----newDescs = (AllocateDesc *) realloc(allocatedDescs,
											newMax * sizeof(AllocateDesc));
----allocatedDescs = newDescs;
----maxAllocatedDescs = newMax;
----return true;
--return false;
```