## Front-end tooling 관련 정리 


gulp.watch사용시 windows 에서 cpu를 많이 소모하는경우가 있는데 이럴땐 아래와 같이 pooling interval을 어느정도 늘려주면 된다. 

```
gulp.watch(['/src/**/*.js'], {
    interval:500 // 기본값은 100으로 설정되어있음
}, ['build:js']);
```
