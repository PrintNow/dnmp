## 提交镜像

```shell
# nginx 指的是文件夹，该文件夹下面有 Dockerfile 文件
docker build -t shine09/nginx:1.21.0 nginx

# 将容器提交镜像<如果你是直接构建的，请直接往下看打标签>
docker commit -a "shine09" -m "Nginx 1.21.0 版本，包括了 http-concat 扩展" 361dfd70ca21 nginx:1.21.0

# 打标签
docker tag 361dfd70ca21 shine09/nginx:1.21.0

# 打好标签推送
docker push shine09/nginx:1.21.0
```

## 复制容器里的文件到宿主机

### PHP 7.0

```shell
# aed3145c6393 是 CONTAINER ID
export PHP_70_CONTAINER_ID=aed3145c6393

docker cp $PHP_70_CONTAINER_ID:/usr/local/etc/php/php.ini-development ./php/70/php.ini
docker cp $PHP_70_CONTAINER_ID:/usr/local/etc/php-fpm.conf./php/70/php-fpm.conf
docker cp $PHP_70_CONTAINER_ID:/usr/local/etc/php-fpm.d ./php/70/php-fpm.d

# 删除环境变量
unset PHP_70_CONTAINER_ID
```

### Nginx

```shell
# e1e393df28c6 是 CONTAINER ID
export NGINX_CONTAINER_ID=e1e393df28c6

docker cp $NGINX_CONTAINER_ID:/etc/nginx/fastcgi_params /Users/chuwen/Docker/dnmp-new/nginx/fastcgi_params
docker cp $NGINX_CONTAINER_ID:/etc/nginx/nginx.conf /Users/chuwen/Docker/dnmp-new/nginx/nginx.conf
docker cp $NGINX_CONTAINER_ID:/etc/nginx/conf.d /Users/chuwen/Docker/dnmp-new/nginx/conf.d

# 删除环境变量
unset NGINX_CONTAINER_ID
```

### MySQL

```shell
export MYSQL_CONTAINER_ID=59565b9c97ec
docker cp $MYSQL_CONTAINER_ID:/etc/mysql /Users/chuwen/Docker/dnmp-new/mysql
unset MYSQL_CONTAINER_ID
```

## 构建镜像

```shell
# Nginx
docker build -t shine09/nginx:1.21.0 nginx

# PHP-fpm 7.0
docker build -t shine09/php-fpm:70 php/70

# PHP-fpm 7.3
docker build -t shine09/php-fpm:73 php/73
export PHP_73_CONTAINER_ID=8e93ea94ae1c
docker cp $PHP_73_CONTAINER_ID:/usr/local/etc/php/php.ini-development   php/73/php.ini
docker cp $PHP_73_CONTAINER_ID:/usr/local/etc/php-fpm.conf              php/73/php-fpm.conf
docker cp $PHP_73_CONTAINER_ID:/usr/local/etc/php-fpm.d                 php/73/php-fpm.d
unset PHP_73_CONTAINER_ID
```