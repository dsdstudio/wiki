Linux 에서 프로세스가 사용중인 파일 목록 볼수있는방법 

```
$ lsof -a -p <pid>
root@memestudio:~# lsof -a -p 1496
COMMAND  PID  USER   FD   TYPE             DEVICE SIZE/OFF    NODE NAME
mysqld  1496 mysql  cwd    DIR              252,0     4096 2367706 /var/lib/mysql
mysqld  1496 mysql  rtd    DIR              252,0     4096       2 /
mysqld  1496 mysql  txt    REG              252,0 16060128 3802446 /usr/sbin/mysqld
mysqld  1496 mysql  mem    REG              252,0   101240  393243 /lib/x86_64-linux-gnu/libresolv-2.19.so
mysqld  1496 mysql  mem    REG              252,0    22952  393329 /lib/x86_64-linux-gnu/libnss_dns-2.19.so
mysqld  1496 mysql  DEL    REG               0,11            12650 /[aio]
```

이는 결국 `/proc/pid/fd` 와 동일

```
$ ls -l /proc/PID/fd

합계 0
lrwx------ 1 root root 64  1월 17 19:03 0 -> /dev/null
l-wx------ 1 root root 64  1월 17 19:03 1 -> /var/log/mysql/error.log
lrwx------ 1 root root 64  1월 17 19:03 10 -> /var/lib/mysql/ib_logfile1
lrwx------ 1 root root 64  1월 17 19:03 100 -> /var/lib/mysql/mysql/time_zone_transition_type.MYD
lrwx------ 1 root root 64  1월 17 19:03 11 -> /var/lib/mysql/mysql/innodb_index_stats.ibd
```



관련 링크

http://www.cyberciti.biz/tips/linux-procfs-file-descriptors.html
