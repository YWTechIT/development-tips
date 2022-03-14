# nginx-tips

### 📍 nginx: [emerg] cannot load certificate  BIO_new_file() failed SSL 오류 해결하기
`Docker-compose`로 배포하는 과정에 다음과 같은 오류로 인해 배포사이트에 `SSL`을 적용할 수 없었다.

```
nginx: [emerg] cannot load certificate "/etc/nginx/ssl/live/<domain>/fullchain.pem": BIO_new_file() failed (SSL: error:02001002:system library:fopen:No such file or directory:fopen('/etc/nginx/ssl/live/<domain>/fullchain.pem','r') error:2006D080:BIO routines:BIO_new_file:no such file)
```

![](https://images.velog.io/images/abcd8637/post/7dcaffcb-05c3-4797-b8b8-808f0a8824fe/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%2010.55.35.png)

나는 `letsencrypt`에서 `SSL` 인증키를 발급받았고 인증서의 만료 기간이 지나지 않았는데, `Docker`로 새로운 `container`를 생성하다보니 이전에 있던 `container`를 모두 삭제하여(기록도 안 남게 `rm -rf`로..) 기존에 `certbot`과 관련한 파일들은 말끔하게 사라졌다. `SSL`을 적용하려면 인증서를 다시 처음부터 발급해야하는 상황인데, 너무 많은 시도로 인해 새로 발급 받으려면 7일을 기다려야했다. (<a href='https://ywtechit.tistory.com/449'>too many certificates</a>)

다른 방법이 없는지 한참을 고민 끝에 `VM ware` 가상환경 파일을 이것저것 둘러보니 `/etc/letsencrypt/` 폴더에 다음과 같은 파일들이 있었다. 처음에 이것들이 무슨 파일인지 몰랐는데, 생각해보니까 `letsencrypt`에서 인증서를 발급받으면 다음과 같은 문장을 봤던 기억이 있었다.

```
Congratulations! Your certificate and chain have been saved at:
/etc/letsencrypt/live/<domain>/fullchain.pem
Your key file has been saved at:
/etc/letsencrypt/live/<domain>/privkey.pem
```

![](https://images.velog.io/images/abcd8637/post/4abd4917-8486-4a33-94d2-0db12948eb60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-11%2012.08.27.png)

호다닥 `/etc/letsencrypt/live/`으로 들어가니까 인증서 관련 파일들이 있었다. (`docker container`를 생성하면 나오는 환경이 아닌 `SSH`환경의 경로이다.)

그래서 가상환경의 `certbot/conf/`에서 `sudo rm -rf conf/`명령어를 사용해 `conf/` 폴더를 제거하고 `/etc/letsencrypt/`에 있는 파일들을 모두 `certbot/conf/` 폴더 내부로 옮겼다. 이때 사용한 명령어는 `sudo cp -r * <붙여넣을 경로>`다. 주의 할점은 `cp -r`로만 복사하면 `cert.pem`, `fullchain.pem`처럼 `.pem`파일은 제대로 복사가 되지 않는다. 명령어에 `*`를 붙여주자.

다시 실행하면 오류가 뜨지 않고 정상적으로 `SSL`로 진입하는 모습을 볼 수 있다. 

![](https://images.velog.io/images/abcd8637/post/2eb21197-2a41-4b09-b7ff-f9d9eda7ef9c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%2010.56.12.png)

여담으로 `docker-compose`로 `Nginx`를 실행하는 경우 `Nginx`를 재시작해줘야 수정사항이 반영된다. `docker-compose restart nginx`를 입력하여 `Nginx`를 재시작해주면 `Restarting git-farm_nginx_1 ... done` 이란 문장이 나타난다.

이렇게 글로 정리해보니 난이도가 있는 작업은 아니었는데, 마땅한 해결책이 없어 이것저것 시도한것이 (일명 삽질) 오래걸렸던 것 같다.
나와 같은 문제로 고생하고 있는 분들에게 조금이나마 도움이 되었으면 좋겠고 `Nginx.conf` 코드를 올리며 글을 마치겠다.

```bash
# nginx.conf
user nginx; 
worker_processes 1; 

events {
    worker_connections 1024;
}

http {
    access_log  /var/log/nginx/access.log;
    error_log   /var/log/nginx/error.log;

    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    server {
        listen 80;
        server_name <domain>;
        root /usr/share/nginx/html/;

        location ~ /.well-known/acme-challenge {
            allow all;
            root /usr/share/nginx/html/letsencrypt;
        }

        location / {
            return 301 <domain>$request_uri;
        }
    }

    server {
        listen 443 ssl;
        server_name <domain>;
        root /usr/share/nginx/html/;
        server_tokens off;

        ssl_certificate /etc/nginx/ssl/live/<domain>/fullchain.pem;
        ssl_certificate_key /etc/nginx/ssl/live/<domain>/privkey.pem;
        ssl_dhparam /etc/nginx/dhparam/dhparam-2048.pem;

        ssl_buffer_size 8k;
        ssl_protocols TLSv1.2 TLSv1.1 TLSv1;
        ssl_prefer_server_ciphers on;
        ssl_ciphers ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DH+3DES:!ADH:!AECDH:!MD5;

        location / {
           try_files $uri /index.html;
        }

        location /api {
            proxy_pass http://backend:7777;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
}
```

---
### 📍 the "ssl" directive is deprecated, use the "listen ...
`Docker`를 이용해 배포 환경에서 `nginx.conf`를 세팅하고 실행을 하는데 다음과 같은 경고문이 떴다.

![](https://images.velog.io/images/abcd8637/post/2cba1c15-2d3a-4eb4-8a5a-232c8a386eb6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-11%2009.57.48.png)

```
nginx_1    | 2022/03/10 22:11:52 [warn] 1#1: the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /etc/nginx/nginx.conf:35

nginx_1    | nginx: [warn] the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /etc/nginx/nginx.conf:35
```

`Error`는 아니기에 정상적으로 실행은 되지만, 자꾸 거슬려서 해결하고 싶었다. 결론적으로 `Nginx`의 버전이 `1.15.0` 이상이라면 `ssl on;` 또는 `ssl off;` 코드를 제거하고 `listen`에 `ssl`을 붙여준다.

```bash
# before
listen 443;
ssl on;

# after
listen 443 ssl;
```

reference 
1. http://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl
2. https://stackoverflow.com/questions/51703109/nginx-the-ssl-directive-is-deprecated-use-the-listen-ssl
3. https://robiokidenis.medium.com/how-to-fix-nginx-warn-the-ssl-directive-is-deprecated-use-the-listen-ssl-6dfaa2356abc