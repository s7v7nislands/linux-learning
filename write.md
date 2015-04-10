```
linux system call:
write -> sys_write -> vfs_write # vfs, get the position of file offset.
                             ->
                             struct file_operations                     # detail fs
                             ->
                             fs.aio_write                               # different between the fs
                             or
                             generic_file_aio_write: lock mutex         # in mm/filemap.c file.
                             generic_write_checks                       # adjust writing position or amount of bytes to write.
                             i_read_size()                              # adjust writing position when file is opened with O_APPEND
                      vfs_write                                         #vfs, set the position of file offset.
```

---

note

```
1. the write call will lock inode.i_mutex to be sure write is atomic.
2. when file is opened with O_APPEND, the call will read the file size again to be sure the writing file is serialable.
```
