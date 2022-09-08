#1.OpenTemporaryFileInTablespace

```
OpenTemporaryFileInTablespace
--TempTablespacePath
----snprintf(path, MAXPGPATH, "pg_tblspc/%u/%s/%s",
				 tablespace, TABLESPACE_VERSION_DIRECTORY,
				 PG_TEMP_FILES_DIR);
--snprintf(tempfilepath, sizeof(tempfilepath), "%s/%s%d.%ld",
			 tempdirpath, PG_TEMP_FILE_PREFIX, MyProcPid, tempFileCounter++);
--PathNameOpenFile
----PathNameOpenFilePerm				 
```