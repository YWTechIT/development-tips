# docker-tips

### ๐ docker-compose dev, prod ๋ชจ๋๋ก ๋ฆฌํฉํ ๋งํ๊ธฐ
`docker-compose`๋ฅผ ์ฌ์ฉํ์ฌ `nginx`๋ฅผ ๋ฐฐํฌ ํ  ๋ ๋งค๋ฒ ๊ฐ์ ํ๊ฒฝ์์ `SSL`์ ์ ์ฉํ๋ ๊ณผ์ ์ ๊ฑฐ์ณ์ผํ๋ค. ์๋ฅผ ๋ค์ด `nginx.conf`์ `server`์ฝ๋๋ฅผ `80`์์ `443`์ผ๋ก ๋ฐ๊พธ๊ณ  ์ธ์ฆ์์ ๊ฒฝ๋ก๋ฅผ ์ถ๊ฐํ๋ ๊ณผ์ ๊ณผ `docker-compose - volumes`์ `letsencrypt` ์ธ์ฆ์๋ฅผ ๋ฃ๋ ๊ณผ์ ์ด ํฌํจ๋๋ค. ํ์ง๋ง ๋งค๋ฒ `SSL`์ ์ ์ฉํ๋ ๊ณผ์ ์ ๊ฑฐ์น๋ค๋ณด๋ ๊ท์ฐฎ์๋ค. ๊ทธ๋์ `SSL`์ ์ ์ฉํ ๋ฒ์ ๊ณผ `SSL`์ ์ ์ฉํ์ง ์์ ๋ฒ์ ์ผ๋ก ๋๋๋ฉด ์ด๋จ๊น?๋ผ๋ ์๊ฐ์ ํ๋ค. 

<a href='https://docs.docker.com/compose/reference/#use--f-to-specify-name-and-path-of-one-or-more-compose-files'>docker</a> ๊ณต์๋ฌธ์๋ฅผ ์ฐพ์๋ณด๋ `-f` ๋ช๋ น์ด๋ฅผ ์ฌ์ฉํ์ฌ ํน์  ํ์ผ์ ๋น๋ ํ  ์ ์๋ค๊ณ  ํ๋ค. ๋ง์ฝ, ์ฌ๋ฌ๊ฐ์ `docker-compose` ํ์ผ์ ๋น๋ํ๊ณ  ์ถ์ผ๋ฉด `-f`๋ฅผ ์ฌ๋ฌ๋ฒ ์ฌ์ฉํ๋ฉด ๋๊ณ  `-f`๋ฅผ ์ฌ์ฉํ ์์๋๋ก ๋น๋ํ๋ค๊ณ  ๋์์์๋ค. ๋ `docker-compose`์ `service` ์ด๋ฆ์ด ๊ฐ๋ค๋ฉด ์ด์  ํ์ผ์ `override`ํ๋ค๊ณ  ๋ช์๋์ด์๋ค.

๊ทธ๋์ ํ์ผ ๊ตฌ์กฐ๋ฅผ ๊ธฐ์กด์ `docker-compose.yml` ํ๋๋ง ์ฌ์ฉํ๋ค๋ฉด `docker-compose.yml`, `docker-compose-dev.yml`, `docker-compose-prod.yml`์ฒ๋ผ ํ์ผ์ 3๊ฐ๋ก ๋๋์๊ณ , ๊ณตํต์ผ๋ก ์ ์ฉํ๋ ์ฝ๋๋ `docker-compose.yml` ํ์ผ์ ์์ฑํ๊ณ  `SSL`์ ์ ์ฉํ์ง ์๋ ๋ฒ์ ์ `docker-compose-dev.yml`์ ์ ์ฅํ๊ณ , `SSL`์ ์ ์ฉํ๋ ๋ฒ์ ์ `docker-compose-prod.yml`์ ์ ์ฅํ๋ค. ๊ทธ๋ฌ๋ฉด ๊ณตํต์ผ๋ก ์์ฑํ ํ์ผ์ `dev` ๋ฒ์ ๊ณผ `prod` ๋ฒ์ ์ด `override`๋๋๊น ๋ช๋ น์ด๋ง ๋ฐ๊ฟ์ฃผ๋ฉด ํด๊ฒฐ๋๋ ๊ฒ์ด๋ค. ๋ง์ฐฌ๊ฐ์ง๋ก `nginx`์ `volume` ๋ํ `SSL`์ ์ ์ฉํ  ๋์ `nginx.conf`์ `SSL`์ ์ ์ฉํ์ง ์์ ๋์ `nginx.dev.conf`๋ก ๋๋์ด ์ ์ฉํ๋๋ ๊ฐ์ํ๊ฒฝ์์ ํ์ผ์ ์์ ํ๋ ๋ฒ๊ฑฐ๋ก์์ ๋ ์ ์์๋ค.

`doc`์ ๋ฐ๋ผ ์์ํด๋ณด๋ ์ด ๋ฐฉ์์ ํ๋ก ํธ์์ `webpack`์ ์ฌ์ฉํ  ๋ `webpack-merge`๋ฅผ ์ฌ์ฉํ์ฌ `development`์ `production` ๋ชจ๋๋ก ๋๋ ๋ `webpack.config.js`๋ฅผ ์ ๊ฑฐํ๊ณ  `webpack.common.js`, `webpack.dev.js`, `webpack.prod.js`๋ก ๋๋๋ ๊ณผ์ ๊ณผ ๋น์ทํ๋ค๊ณ  ์๊ฐํ๋ค.

๋์ ๊ฐ์ ๋ฌธ์ ๋ก ๊ณ ๋ฏผํ๊ณ  ์๋ ๋ถ๋ค์๊ฒ ๋์์ด ๋์์ผ๋ฉด ์ข๊ฒ ๊ณ , ๋ง์ง๋ง์ผ๋ก ์ค์ ๋ก ์ด๋ป๊ฒ ์ฝ๋๋ฅผ ๋ถํ  ํ๋์ง ๋ช๋ น์ด, `docker-compose`, `nginx` ์ฝ๋๋ฅผ ์ฌ๋ฆฌ๋ฉฐ ๊ธ์ ๋ง์น๊ฒ ๋ค.

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
### ๐ Error: EPERM: operation not permitted, scandir ๊ถํ ์ค๋ฅ
`docker-compose`๋ฅผ ์ฌ์ฉํ๋ค๊ฐ ๊ถํ ๋ฌธ์ ๋ก ๋ค์๊ณผ ๊ฐ์ ์ค๋ฅ๊ฐ ๋ฐ์ํ๋ค.

![](https://images.velog.io/images/abcd8637/post/1f1a13ac-860c-4a8c-82cc-f6a8d76373c9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-02%2010.58.06.png)

๊ถํ๋ฌธ์ ๋ผ๊ณ  ํ๊ธธ๋ `privileged: true`, `build` ๊ฒฝ๋ก ๋ณ๊ฒฝ, `express` ๋ธ๋ ๋ฒ์ ๊ณผ `Backend/Dockerfile` ๋ฒ์  Sync ์ด๊ฒ์ ๊ฒ ํด๋ดค์ง๋ง ๊ถํ๋ฌธ์ ๋ก ์คํ๋์ง ์์๋ค. ์ ๋ง์ ์ฝ์ง ๋์ `MAC` ์์คํ ํ๊ฒฝ์ค์  - ๋ณด์ ๋ฐ ๊ฐ์ธ ์ ๋ณด ๋ณดํธ - ์ ์ฒด ๋์คํฌ ์ ๊ทผ ๊ถํ - `Docker` ์ถ๊ฐํ๋๊น ํด๊ฒฐ๋์๋ค.

![](https://images.velog.io/images/abcd8637/post/ff78948a-8408-4581-a831-205b1c59c22a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-03%2013.23.21.png)

---
### ๐ Let's Encrypt: Error creating new order :: too many certificates ์ค๋ฅ
`Docker-compose`๋ฅผ ์ด์ฉํ์ฌ ์๋ฒ๋ฅผ ๋ฐฐํฌ ํ  ๋ `SSL(Secure Sockets Layer)`์ธ์ฆ์ ๋ฐ๊ธฐ ์ํด ๋ฒ์ฉ์ ์ผ๋ก ์ฌ์ฉํ๋ ๋ฌด๋ฃ ์ธ์ฆ๊ธฐ๊ด์ธ `Let's Encrypt`์ ์ด์ฉํ๋ค๊ฐ ํ์คํธ ์์ํ๋๋ผ ์ธ์ฆ์๋ฅผ ์ฌ๋ฌ๋ฒ ์ฌ๋ฐ๊ธ ๋ฐ์๋๋ ๋ค์๊ณผ ๊ฐ์ ์ค๋ฅ๊ฐ ๋จ๋ฉด์ ๋ ์ด์ ์ธ์ฆ์๋ฅผ ๋ฐ๊ธํด์ฃผ์ง ์์๋ค.

![](https://images.velog.io/images/abcd8637/post/86a4686c-4340-4146-af72-01b56b012f80/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-04%2014.19.09.png)

์์ด๋ฅผ ์ฝ์ด๋ณด๋ฉด ์ ์ ์๋ฏ์ด ๋๋ฌด ๋ง์ด ์ธ์ฆ์๋ฅผ ์์ฒญํ์ฌ `Let's Encrypt`์์ ๋ ์ด์ ์์ฒญํ์ง ๋ชปํ๊ฒ ์ ํ์ํจ ๊ฒ์ธ๋ฐ, `limit-rate`์ `The main limit is Certificates per Registered Domain (50 per week)`๋ผ๊ณ  ์์ฑ๋์ด์๋๊ฒ์ผ๋ก ๋ณด์ ์ฃผ๋น 50๋ฒ๊น์ง ์์ฒญ ํ  ์ ์๋ ๊ฒ์ ์ ์ ์๊ณ  50๋ฒ์ด ๋์ด๊ฐ๋ฉด ์ฌ์ง์ฒ๋ผ 1์ฃผ์ผ ๋์ ์์ฒญ ํ  ์ ์๋๋ก `limit`์ด ๊ฑธ๋ฆฌ๊ฒ ๋๋ค. 

๊ฒฐ๋ก ์ ์ผ๋ก, ๋ ์ด์ ์ธ์ฆ์๋ฅผ ๋ฐ๊ธ ๋ฐ์ ์ ์๊ธฐ ๋๋ฌธ์ ๊ธฐ์กด ์ธ์ฆ์๋ฅผ ์ฌ์ฉํด์ผ ํ๋๋ฐ, ๋๋ ์ธ์ฆ์๋ฅผ ๋ฐฑ์ํด๋์ง ์๊ณ  ํ์คํธํ๊ธฐ ๋๋ฌธ์ 1์ฃผ์ผ๋์ ๊ธฐ๋ค๋ ค์ผํ๋ ์ด๋ง์ ๋ณด๊ฒ ๋์๋ค. ์ด๋ง์ ๋ณด๊ณ ๋์ ์๊ฒ ๋ ๊ฒ์ด์ง๋ง ๋ง์ฝ, ์ธ์ฆ์๋ฅผ 50๋ฒ์ ๋๊ฒ ๋ฐ๊ธ ๋ฐ์์ผํ๋ ์ํฉ์ด๋ฉด `staging-environment`์์๋ `--dry-run` ์ปค๋งจ๋๋ฅผ ์๋ ฅํ์ฌ ๋ฐ๊ธ๋ฐ๋๋ก ํ์. `production-environment`์๋ ๋ค๋ฅด๊ฒ ๋ค์์ฒ๋ผ ์ฌ๋ฐ๊ธ์ ๊ฑฑ์ ํ์ง ์์๋ ๋๋ ํ์๋งํผ ๋์ด๋๋ค. ํ๋จ์ `code-block`์ `staging-environment`๊ฐ ๋ฌด์์ธ์ง ์๋ ค์ค๋ค.

```
1. ๋ฑ๋ก ๋ ๋๋ฉ์ธ ๋น ์ธ์ฆ์ ์ ํ๋๋ ์ฃผ๋น 30,000์๋๋ค.
2. ์ค๋ณต ์ธ์ฆ์ ํ๋๋ ์ฃผ๋น 30,000์๋๋ค.
3. ์คํจํ ์ ํจ์ฑ ๊ฒ์ฌ ์ ํ์ ์๊ฐ๋น 60์๋๋ค.
4. ๊ณ์  ๋น IP ์ฃผ์ ์ ํ์ IP ๋น 3์๊ฐ, 3์๊ฐ๋น 50๊ณ์ ์๋๋ค.
```

๋ฌผ๋ก , `staging-environment`๋ก ๋ฐ๊ธ๋ฐ๋๊ฒ๋ ์ค์ํ์ง๋ง ๋ฌด์๋ณด๋ค `production-environment`๋ก ํ๋ฒ ๋ฐ๊ธ ๋ฐ์ ์ธ์ฆ์๋ฅผ ๋ฐฑ์ํด ๋๊ณ  ๋ ์ด์ ๋ฐ๊ธ์ด ์๋๋ฉด ๋ฐฑ์ ์ธ์ฆ์๋ฅผ ์ฌ์ฉํ์. `production-environment`์์ ๋ฐ์ ์ธ์ฆ์๋ ๋ณดํต `./certbot/live/<domain>` ํด๋ ์์ ์ ์ฅ๋์ด์๋ค. ๋ง์ฝ, `./certbot/`์ `live` ํด๋๊ฐ ์๋ค๋ฉด ๋๋ฉ์ธ ์ธ์ฆ์๋ฅผ ๋ฐ์ง ์์ ์ํ์ด๋ฏ๋ก ํ๋จ์ `docker-compose` ์ฝ๋๋ฅผ ์ฐธ๊ณ ํ์ฌ `docker-compose up` ํ  ๋ ์ธ์ฆ์๋ฅผ ๋ฐ์์ค์.

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