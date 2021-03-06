# nginx-tips

### π nginx: [emerg] cannot load certificate  BIO_new_file() failed SSL μ€λ₯ ν΄κ²°νκΈ°
`Docker-compose`λ‘ λ°°ν¬νλ κ³Όμ μ λ€μκ³Ό κ°μ μ€λ₯λ‘ μΈν΄ λ°°ν¬μ¬μ΄νΈμ `SSL`μ μ μ©ν  μ μμλ€.

```
nginx: [emerg] cannot load certificate "/etc/nginx/ssl/live/<domain>/fullchain.pem": BIO_new_file() failed (SSL: error:02001002:system library:fopen:No such file or directory:fopen('/etc/nginx/ssl/live/<domain>/fullchain.pem','r') error:2006D080:BIO routines:BIO_new_file:no such file)
```

![](https://images.velog.io/images/abcd8637/post/7dcaffcb-05c3-4797-b8b8-808f0a8824fe/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%2010.55.35.png)

λλ `letsencrypt`μμ `SSL` μΈμ¦ν€λ₯Ό λ°κΈλ°μκ³  μΈμ¦μμ λ§λ£ κΈ°κ°μ΄ μ§λμ§ μμλλ°, `Docker`λ‘ μλ‘μ΄ `container`λ₯Ό μμ±νλ€λ³΄λ μ΄μ μ μλ `container`λ₯Ό λͺ¨λ μ­μ νμ¬(κΈ°λ‘λ μ λ¨κ² `rm -rf`λ‘..) κΈ°μ‘΄μ `certbot`κ³Ό κ΄λ ¨ν νμΌλ€μ λ§λνκ² μ¬λΌμ‘λ€. `SSL`μ μ μ©νλ €λ©΄ μΈμ¦μλ₯Ό λ€μ μ²μλΆν° λ°κΈν΄μΌνλ μν©μΈλ°, λλ¬΄ λ§μ μλλ‘ μΈν΄ μλ‘ λ°κΈ λ°μΌλ €λ©΄ 7μΌμ κΈ°λ€λ €μΌνλ€. (<a href='https://ywtechit.tistory.com/449'>too many certificates</a>)

λ€λ₯Έ λ°©λ²μ΄ μλμ§ νμ°Έμ κ³ λ―Ό λμ `VM ware` κ°μνκ²½ νμΌμ μ΄κ²μ κ² λλ¬λ³΄λ `/etc/letsencrypt/` ν΄λμ λ€μκ³Ό κ°μ νμΌλ€μ΄ μμλ€. μ²μμ μ΄κ²λ€μ΄ λ¬΄μ¨ νμΌμΈμ§ λͺ°λλλ°, μκ°ν΄λ³΄λκΉ `letsencrypt`μμ μΈμ¦μλ₯Ό λ°κΈλ°μΌλ©΄ λ€μκ³Ό κ°μ λ¬Έμ₯μ λ΄€λ κΈ°μ΅μ΄ μμλ€.

```
Congratulations! Your certificate and chain have been saved at:
/etc/letsencrypt/live/<domain>/fullchain.pem
Your key file has been saved at:
/etc/letsencrypt/live/<domain>/privkey.pem
```

![](https://images.velog.io/images/abcd8637/post/4abd4917-8486-4a33-94d2-0db12948eb60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-11%2012.08.27.png)

νΈλ€λ₯ `/etc/letsencrypt/live/`μΌλ‘ λ€μ΄κ°λκΉ μΈμ¦μ κ΄λ ¨ νμΌλ€μ΄ μμλ€. (`docker container`λ₯Ό μμ±νλ©΄ λμ€λ νκ²½μ΄ μλ `SSH`νκ²½μ κ²½λ‘μ΄λ€.)

κ·Έλμ κ°μνκ²½μ `certbot/conf/`μμ `sudo rm -rf conf/`λͺλ Ήμ΄λ₯Ό μ¬μ©ν΄ `conf/` ν΄λλ₯Ό μ κ±°νκ³  `/etc/letsencrypt/`μ μλ νμΌλ€μ λͺ¨λ `certbot/conf/` ν΄λ λ΄λΆλ‘ μ?κ²Όλ€. μ΄λ μ¬μ©ν λͺλ Ήμ΄λ `sudo cp -r * <λΆμ¬λ£μ κ²½λ‘>`λ€. μ£Όμ ν μ μ `cp -r`λ‘λ§ λ³΅μ¬νλ©΄ `cert.pem`, `fullchain.pem`μ²λΌ `.pem`νμΌμ μ λλ‘ λ³΅μ¬κ° λμ§ μλλ€. λͺλ Ήμ΄μ `*`λ₯Ό λΆμ¬μ£Όμ.

λ€μ μ€ννλ©΄ μ€λ₯κ° λ¨μ§ μκ³  μ μμ μΌλ‘ `SSL`λ‘ μ§μνλ λͺ¨μ΅μ λ³Ό μ μλ€. 

![](https://images.velog.io/images/abcd8637/post/2eb21197-2a41-4b09-b7ff-f9d9eda7ef9c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%2010.56.12.png)

μ¬λ΄μΌλ‘ `docker-compose`λ‘ `Nginx`λ₯Ό μ€ννλ κ²½μ° `Nginx`λ₯Ό μ¬μμν΄μ€μΌ μμ μ¬ν­μ΄ λ°μλλ€. `docker-compose restart nginx`λ₯Ό μλ ₯νμ¬ `Nginx`λ₯Ό μ¬μμν΄μ£Όλ©΄ `Restarting git-farm_nginx_1 ... done` μ΄λ λ¬Έμ₯μ΄ λνλλ€.

μ΄λ κ² κΈλ‘ μ λ¦¬ν΄λ³΄λ λμ΄λκ° μλ μμμ μλμλλ°, λ§λν ν΄κ²°μ±μ΄ μμ΄ μ΄κ²μ κ² μλνκ²μ΄ (μΌλͺ μ½μ§) μ€λκ±Έλ Έλ κ² κ°λ€.
λμ κ°μ λ¬Έμ λ‘ κ³ μνκ³  μλ λΆλ€μκ² μ‘°κΈμ΄λλ§ λμμ΄ λμμΌλ©΄ μ’κ² κ³  `Nginx.conf` μ½λλ₯Ό μ¬λ¦¬λ©° κΈμ λ§μΉκ² λ€.

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
### π the "ssl" directive is deprecated, use the "listen ...
`Docker`λ₯Ό μ΄μ©ν΄ λ°°ν¬ νκ²½μμ `nginx.conf`λ₯Ό μΈννκ³  μ€νμ νλλ° λ€μκ³Ό κ°μ κ²½κ³ λ¬Έμ΄ λ΄λ€.

![](https://images.velog.io/images/abcd8637/post/2cba1c15-2d3a-4eb4-8a5a-232c8a386eb6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-11%2009.57.48.png)

```
nginx_1    | 2022/03/10 22:11:52 [warn] 1#1: the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /etc/nginx/nginx.conf:35

nginx_1    | nginx: [warn] the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /etc/nginx/nginx.conf:35
```

`Error`λ μλκΈ°μ μ μμ μΌλ‘ μ€νμ λμ§λ§, μκΎΈ κ±°μ¬λ €μ ν΄κ²°νκ³  μΆμλ€. κ²°λ‘ μ μΌλ‘ `Nginx`μ λ²μ μ΄ `1.15.0` μ΄μμ΄λΌλ©΄ `ssl on;` λλ `ssl off;` μ½λλ₯Ό μ κ±°νκ³  `listen`μ `ssl`μ λΆμ¬μ€λ€.

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