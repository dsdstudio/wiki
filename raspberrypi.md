Raspberry pi Rev.B + Archlinux Cookbook 

## exfat mount

	$ pacman -Sy fuse-exfat
	$ mount.exfat-fuse /dev/sdXn /mnt/exfat


## Samba 설정 

	설치 
	# pacman -Sy samba 
	# cp /etc/samba/smb.conf.default /etc/samba/smb.conf

	사용자 추가 
	# smbpasswd -a userid

	데몬에 등록 (smbd.service, nmbd.service)
	# systemctl enable smbd.service 

	서비스 시작 
	# systemctl start smbd.service

### smb.conf 설정 

	[share]
        path = /media/share
		valid users = bhkim
		public = no
		writable = yes
		create mask = 0777

## mac 에서 마운트 

	$ mount_smbfs //userid:userpw@ipaddr/share /targetdir

## 참고자료 


- [https://wiki.archlinux.org/index.php/Samba#Server](https://wiki.archlinux.org/index.php/Samba#Server)
- [daemon list, 및 시스템 등록 서비스명](https://wiki.archlinux.org/index.php/Daemons_List)
