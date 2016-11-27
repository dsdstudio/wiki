i## docker cookbook 


### docker-compose 에서도 환경변수를 사용하는것이 가능해졌다. 

단, docker-compose 1.5이상에서만 사용가능

환경변수를 넘길수 있는방법은 총 3가지이다. 

- .env파일에 `Key=Value` 형식으로 명시 
- `export KEY=VAL` 전통적인 환경변수 세팅 
- `docker-compose run -e KEY=VAL` 명령행 인자로 inline형태로 실행

docker-compose.yml 
```
nginx:
  env_file:
    - vars.env // 환경변수파일을 직접 명시하는것도 가능 없는경우에는 .env 파일을 찾는다.
  extra_hosts:
    - "dsdstudio.net:${HOST_IP}"
```


옛날처럼 `envsubst < .... > ...` 와 같은 삽질을 더이상 하지않아도 된다. 

#### reference 

- https://docs.docker.com/compose/environment-variables/
