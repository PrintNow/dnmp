FROM php:7.0-fpm AS build

#MAINTAINER 维护者信息
MAINTAINER shine09 i@nowtime.cc

ENV INSTALL_PACKGE='pdo pdo_mysql mysqli'
ENV REDIS_VERSION='5.3.4'

# 因为在中国可能会打不开 pecl.php.net 所以需要挂代理
#RUN printf "\n" | curl -x socks5://192.168.1.64:7890 \
RUN printf "\n" | curl \
        "https://pecl.php.net/get/redis-$REDIS_VERSION.tgz" \
        -o redis-$REDIS_VERSION.tgz \
        && pecl install redis-$REDIS_VERSION.tgz \
        && rm -rf redis-$REDIS_VERSION.tgz \
        && docker-php-ext-install $INSTALL_PACKGE \
        && rm -rf /tmp/pear \
        && docker-php-ext-enable $INSTALL_PACKGE redis

EXPOSE 9000
CMD ["php-fpm"]
