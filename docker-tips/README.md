# docker-tips

### 📍 docker-compose dev, prod 모드로 리팩토링하기
`docker-compose`를 사용하여 `nginx`를 배포 할 때 매번 가상 환경에서 `SSL`을 적용하는 과정을 거쳐야했다. 예를 들어 `nginx.conf`의 `server`코드를 `80`에서 `443`으로 바꾸고 인증서의 경로를 추가하는 과정과 `docker-compose - volumes`에 `letsencrypt` 인증서를 넣는 과정이 포함됐다. 하지만 매번 `SSL`을 적용하는 과정을 거치다보니 귀찮았다. 그래서 `SSL`을 적용한 버전과 `SSL`을 적용하지 않은 버전으로 나누면 어떨까?라는 생각을 했다. 

<a href='https://docs.docker.com/compose/reference/#use--f-to-specify-name-and-path-of-one-or-more-compose-files'>docker</a> 공식문서를 찾아보니 `-f` 명령어를 사용하여 특정 파일을 빌드 할 수 있다고 했다. 만약, 여러개의 `docker-compose` 파일을 빌드하고 싶으면 `-f`를 여러번 사용하면 되고 `-f`를 사용한 순서대로 빌드한다고 나와있었다. 또 `docker-compose`의 `service` 이름이 같다면 이전 파일에 `override`한다고 명시되어있다.

그래서 파일 구조를 기존에 `docker-compose.yml` 하나만 사용했다면 `docker-compose.yml`, `docker-compose-dev.yml`, `docker-compose-prod.yml`처럼 파일을 3개로 나누었고, 공통으로 적용하는 코드는 `docker-compose.yml` 파일에 작성하고 `SSL`을 적용하지 않는 버전은 `docker-compose-dev.yml`에 저장하고, `SSL`을 적용하는 버전은 `docker-compose-prod.yml`에 저장했다. 그러면 공통으로 작성한 파일에 `dev` 버전과 `prod` 버전이 `override`되니까 명령어만 바꿔주면 해결되는 것이다. 마찬가지로 `nginx`의 `volume` 또한 `SSL`을 적용할 때의 `nginx.conf`와 `SSL`을 적용하지 않을 때의 `nginx.dev.conf`로 나누어 적용했더니 가상환경에서 파일을 수정하는 번거로움을 덜 수 있었다.

`doc`을 따라 작업해보니 이 방식은 프론트에서 `webpack`을 사용할 때 `webpack-merge`를 사용하여 `development`와 `production` 모드로 나눌 때 `webpack.config.js`를 제거하고 `webpack.common.js`, `webpack.dev.js`, `webpack.prod.js`로 나누는 과정과 비슷하다고 생각했다.

나와 같은 문제로 고민하고 있는 분들에게 도움이 되었으면 좋겠고, 마지막으로 실제로 어떻게 코드를 분할 했는지 명령어, `docker-compose`, `nginx` 코드를 올리며 글을 마치겠다.

```bash
# before command
docker-compose -f docker-compose.yml up -d --build

# after command
docker-compose -f docker-compose.yml -f docker-compose-prod.yml up -d --build
```

```yaml
# before refactor docker-compose.yml
# docker-compose.yml
version: "3"
services:
  nginx:
    image: nginx
    volumes:
      - ./Frontend/dist/:/usr/share/nginx/html
      - ./Nginx/nginx.conf:/etc/nginx/nginx.conf
    ports:
      - "80:80"
    depends_on:
      - backend

  backend:
    build:
      context: ./Backend
      dockerfile: Dockerfile
    ports:
      - "7777:7777"
    env_file:
      - ./Backend/.env

# after refactor docker-compose.yml
# docker-compose.yml
version: "3"
services:
  backend:
    build:
      context: ./Backend
      dockerfile: Dockerfile
    ports:
      - "7777:7777"
    env_file:
      - ./Backend/.env

# docker-compose-dev.yml
version: "3"
services:
  nginx:
    image: nginx
    volumes:
      - ./Frontend/dist/:/usr/share/nginx/html
      - ./Nginx/nginx-dev.conf:/etc/nginx/nginx.conf
    ports:
      - "1111:80"
    depends_on:
      - backend

  backend:
    build:
      context: .
      args:
        NODE_ENV: development
    volumes:
      - ./:/app
    command: npm run dev

# docker-compose-prod.yml
version: "3"
services:
  nginx:
    image: nginx
    volumes:
      - ./Frontend/dist/:/usr/share/nginx/html
      - ./Nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./dhparam:/etc/nginx/dhparam
      - /etc/letsencrypt:/etc/nginx/ssl
    ports:
      - "80:80"
      - "443:443"
    depends_on:
      - backend

  backend:
    build:
      context: ./Backend
      dockerfile: Dockerfile
      args:
        NODE_ENV: production
    command: npm run deploy
```

```javascript
// nginx.conf
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
            root /usr/share/nginx/html/;
        }

        location / {
            return 301 <domain>;
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

// nginx.dev.conf
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
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name localhost;
        root /usr/share/nginx/html/;

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

reference
1. https://docs.docker.com/compose/reference/#use--f-to-specify-name-and-path-of-one-or-more-compose-files
2. https://docs.docker.com/compose/reference/#specifying-multiple-compose-files
3. https://webpack.kr/guides/production/#setup

---
### 📍 Error: EPERM: operation not permitted, scandir 권한 오류
`docker-compose`를 사용하다가 권한 문제로 다음과 같은 오류가 발생했다.

![](https://images.velog.io/images/abcd8637/post/1f1a13ac-860c-4a8c-82cc-f6a8d76373c9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-02%2010.58.06.png)

권한문제라고 하길래 `privileged: true`, `build` 경로 변경, `express` 노드 버전과 `Backend/Dockerfile` 버전 Sync 이것저것 해봤지만 권한문제로 실행되지 않았다. 수 많은 삽질 끝에 `MAC` 시스템 환경설정 - 보안 및 개인 정보 보호 - 전체 디스크 접근 권한 - `Docker` 추가하니까 해결되었다.

![](https://images.velog.io/images/abcd8637/post/ff78948a-8408-4581-a831-205b1c59c22a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-03%2013.23.21.png)

---
### 📍 Let's Encrypt: Error creating new order :: too many certificates 오류
`Docker-compose`를 이용하여 서버를 배포 할 때 `SSL(Secure Sockets Layer)`인증을 받기 위해 범용적으로 사용하는 무료 인증기관인 `Let's Encrypt`을 이용하다가 테스트 작업하느라 인증서를 여러번 재발급 받았더니 다음과 같은 오류가 뜨면서 더 이상 인증서를 발급해주지 않았다.

![](https://images.velog.io/images/abcd8637/post/86a4686c-4340-4146-af72-01b56b012f80/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-04%2014.19.09.png)

영어를 읽어보면 알 수 있듯이 너무 많이 인증서를 요청하여 `Let's Encrypt`에서 더 이상 요청하지 못하게 제한시킨 것인데, `limit-rate`에 `The main limit is Certificates per Registered Domain (50 per week)`라고 작성되어있는것으로 보아 주당 50번까지 요청 할 수 있는 것을 알 수 있고 50번이 넘어가면 사진처럼 1주일 동안 요청 할 수 없도록 `limit`이 걸리게 된다. 

결론적으로, 더 이상 인증서를 발급 받을 수 없기 때문에 기존 인증서를 사용해야 하는데, 나는 인증서를 백업해두지 않고 테스트했기 때문에 1주일동안 기다려야하는 쓴맛을 보게 되었다. 쓴맛을 보고나서 알게 된 것이지만 만약, 인증서를 50번을 넘게 발급 받아야하는 상황이면 `staging-environment`에서는 `--dry-run` 커맨드를 입력하여 발급받도록 하자. `production-environment`와는 다르게 다음처럼 재발급을 걱정하지 않아도 되는 횟수만큼 늘어난다. 하단의 `code-block`은 `staging-environment`가 무엇인지 알려준다.

```
1. 등록 된 도메인 당 인증서 수 한도는 주당 30,000입니다.
2. 중복 인증서 한도는 주당 30,000입니다.
3. 실패한 유효성 검사 제한은 시간당 60입니다.
4. 계정 당 IP 주소 제한은 IP 당 3시간, 3시간당 50계정입니다.
```

물론, `staging-environment`로 발급받는것도 중요한지만 무엇보다 `production-environment`로 한번 발급 받은 인증서를 백업해 놓고 더 이상 발급이 안되면 백업 인증서를 사용하자. `production-environment`에서 받은 인증서는 보통 `./certbot/live/<domain>` 폴더 안에 저장되어있다. 만약, `./certbot/`에 `live` 폴더가 없다면 도메인 인증서를 받지 않은 상태이므로 하단의 `docker-compose` 코드를 참고하여 `docker-compose up` 할 때 인증서를 받아오자.

```yaml
# docker-compose.yml
version: "3"
services:
  certbot:
    image: certbot/certbot
    command: certonly --webroot --webroot-path=/usr/share/nginx/html/letsencrypt --email <my-email.com> --agree-tos --no-eff-email -d <my-domain.com>
    volumes:
      - ./certbot/conf/:/etc/letsencrypt
      - ./certbot/logs/:/var/log/letsencrypt
      - ./certbot/data:/usr/share/nginx/html/letsencrypt
```

reference
1. <a href='https://letsencrypt.org/ko/docs/rate-limits/'>Let's encrypt docs: rate-limits</a>
2. <a href='https://letsencrypt.org/ko/docs/staging-environment/'>Let's encrypt docs: staging-environment</a>