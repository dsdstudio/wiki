## nginx 로그 필터링 

시나리오 1. /resource 로 들어오는 request 는 access_log 안 남기고싶다.

map directive 를 이용하면 간단하게 해결이 가능하다. 내가 원하는 평가식을 작성하고 `$is_resource` 에 바인딩 시킨다. 

그런 다음 필요한 부분에서 문을 이용해서 적절한 로직을 작성하면 된다. 

`/etc/nginx/conf.d/xxx.net.conf`

	map $request_uri $is_resource {
		default 0;
		~^/resources    1;
	}
	server {
		location / {
			if ($is_resource) {
				access_log off;
			}
			// 블라블라
		}
	}

nginx 재시작 

	# nginx -s reload


## References 

- http://nginx.org/en/docs/http/ngx_http_map_module.html
