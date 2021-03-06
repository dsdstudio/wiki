## Raspberry pi Rev.B + Archlinux Cookbook

### 메모리카드 partition resize 

```
[root@alarmpi ~]# fdisk /dev/mmcblk0

Command (m for help): p

Disk /dev/mmcblk0: 3.7 GiB, 3963617280 bytes, 7741440 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x417ee54b

Device         Boot     Start       End  Blocks  Id System
/dev/mmcblk0p1           2048    186367   92160   c W95 FAT32 (LBA)
/dev/mmcblk0p2         186368   3667967 1740800   5 Extended
/dev/mmcblk0p5         188416   3667967 1739776  83 Linux

Command (m for help): d
Partition number (1,2,5, default 5): 2

Partition 2 has been deleted.

Command (m for help): n

Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): e
Partition number (2-4, default 2): 2
First sector (186368-7741439, default 186368):
Last sector, +sectors or +size{K,M,G,T,P} (186368-7741439, default 7741439):

Created a new partition 2 of type 'Extended' and of size 3.6 GiB.

Command (m for help): n

Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): l

Adding logical partition 5
First sector (188416-7741439, default 188416):
Last sector, +sectors or +size{K,M,G,T,P} (188416-7741439, default 7741439):

Created a new partition 5 of type 'Linux' and of size 3.6 GiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Re-reading the partition table failed.: Device or resource busy

The kernel still uses the old table. The new table will be used at the next reboot or after you run partprobe(8) or kpartx(8).

# reboot 

// 재부팅 후 


[root@alarmpi ~]# resize2fs /dev/mmcblk0p5
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/mmcblk0p5 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/mmcblk0p5 is now 3914496 blocks long.

[root@alarmpi ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        15G  437M   14G   4% /
devtmpfs         87M     0   87M   0% /dev
tmpfs            91M     0   91M   0% /dev/shm
tmpfs            91M  304K   91M   1% /run
tmpfs            91M     0   91M   0% /sys/fs/cgroup
tmpfs            91M     0   91M   0% /tmp
/dev/mmcblk0p1   90M   26M   65M  28% /boot
tmpfs            19M     0   19M   0% /run/user/0
[root@alarmpi ~]#
```

### GPU 메모리 조정 

/boot/config.txt 

	# Headless 형태로 쓰는경우.
    # 기본은 512일때 315, 256일때 128m 사용하도록 설정되어있다.
	gpu_mem_512=16
	gpu_mem_256=16

### Network 설정 

static ip 설정

	# pacman -Syu initscripts
    # shutdown -r now 
    # vi /etc/rc.conf 
    
    # NETWORKING 
    interface=eth0
	address=192.168.0.110
	netmask=255.255.255.0
	broadcast=192.168.0.255
	gateway=192.168.0.1

	# systemctl disable dhcpcd@eth0; netctl enable eth0
    # shutdown -r now

### Samba 설정 

	설치 
	# pacman -Sy samba 
	# cp /etc/samba/smb.conf.default /etc/samba/smb.conf

	사용자 추가 
	# smbpasswd -a userid

	데몬에 등록 (smbd.service, nmbd.service)
	# systemctl enable smbd.service 

	서비스 시작 
	# systemctl start smbd.service


smb.conf 설정 

	[share]
        path = /media/share
		valid users = bhkim
		public = no
		writable = yes
		create mask = 0777

### mac 에서 마운트 

	$ mount_smbfs //userid:userpw@ipaddr/share /targetdir

### 서비스관리 

일반적으로 systemctl (systemd) 를 사용한다.  
이 바이너리가 결국 Centos의 service 와 비슷한 일을 하고있다. 

### 참고자료 


- [https://wiki.archlinux.org/index.php/Samba#Server](https://wiki.archlinux.org/index.php/Samba#Server)
- [daemon list, 및 시스템 등록 서비스명](https://wiki.archlinux.org/index.php/Daemons_List)
