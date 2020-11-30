## SSH key 백업 및 restore 

키 백업후 리스토어시 ssh agent에 등록을 해줘야 ssh client가 해당 private/public key를 인식할 수 있다.

```
# private key ownership 700 
$ chmod 700 ~/.ssh/id_*
$ ssh-add
$ ssh-add -l
```


