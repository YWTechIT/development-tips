# nginx-tips

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