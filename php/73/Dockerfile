FROM php:7.3-fpm AS build

#MAINTAINER 维护者信息
MAINTAINER shine09 i@nowtime.cc

ENV REDIS_VERSION='5.3.4'
ENV XDEBUG_VERSION='3.0.4'

ENV PECL_INSTALL_PACKGE='redis-$REDIS_VERSION.tgz xdebug-$XDEBUG_VERSION.tgz'

ENV INSTALL_PACKGE='pdo pdo_mysql mysqli'
ENV ENABLE_PACKGE='redis xdebug'

# 因为在中国可能会打不开 pecl.php.net 所以需要挂代理
#RUN curl -x socks5://192.168.1.64:7890 "http://xdebug.org/files/xdebug-$XDEBUG_VERSION.tgz" -o xdebug-$XDEBUG_VERSION.tgz && \
#    curl -x socks5://192.168.1.64:7890 "https://pecl.php.net/get/redis-$REDIS_VERSION.tgz" -o redis-$REDIS_VERSION.tgz && \
RUN curl "http://xdebug.org/files/xdebug-$XDEBUG_VERSION.tgz" -o xdebug-$XDEBUG_VERSION.tgz && \
    curl "https://pecl.php.net/get/redis-$REDIS_VERSION.tgz" -o redis-$REDIS_VERSION.tgz && \
    pecl install $PECL_INSTALL_PACKGE && \
    rm -rf $PECL_INSTALL_PACKGE && \
    docker-php-ext-install $INSTALL_PACKGE redis && \
    docker-php-ext-enable $INSTALL_PACKGE $ENABLE_PACKGE

EXPOSE 9000
CMD ["php-fpm"]
