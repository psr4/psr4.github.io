#### 试写一个套 docker-compose 的 lnmp  
- TODO: 换fpm 或给fpm转上mysql扩展
##### 目录结构
```
-docker-compose.yml
-mysqldata
-nginx
    -nginx.conf
-wwwroot
    -index.php
```

##### nginx.conf
```
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.php;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ \.php$ {
        root           /usr/share/nginx/html;
        fastcgi_pass   fpm:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

}

```
##### docker-compose.yml
```
version: "3"
services: 
    php:
        image: docker.io/php:7.2.2-fpm-alpine
        volumes:
            - ./wwwroot:/usr/share/nginx/html
        networks: 
            lnmp:
                aliases:
                    - fpm
            
    nginx: 
        image: docker.io/nginx:latest
        volumes: 
            - ./wwwroot:/usr/share/nginx/html
            - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
        networks:
            lnmp:
                aliases:
                    - nginx
        ports:
            - 80:80
    mysql: 
        image: docker.io/mysql:5.5
        environment:
            - MYSQL_ROOT_PASSWORD=root
        volumes: 
            - ./mysqldata:/var/lib/mysql
        networks:
            lnmp:
                aliases:
                    - mysql

networks: 
    lnmp:
    
```