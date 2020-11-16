## Disk BenchMark

```

root@gram01:/data# dd if=/dev/zero of=benchfile bs=4k count=200000 && sync; rm benchfile
200000+0 records in
200000+0 records out
819200000 bytes (819 MB, 781 MiB) copied, 1.97716 s, 414 MB/s
root@gram01:/data# cd f/
root@gram01:/data/f# dd if=/dev/zero of=benchfile bs=4k count=200000 && sync; rm benchfile
200000+0 records in
200000+0 records out
819200000 bytes (819 MB, 781 MiB) copied, 8.02269 s, 102 MB/s
```
