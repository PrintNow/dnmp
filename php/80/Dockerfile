FROM php:8.0-fpm AS build

#MAINTAINER 维护者信息
MAINTAINER shine09 i@nowtime.cc

ENV INSTALL_PACKGE='pdo pdo_mysql mysqli redis'

# 因为在中国可能会打不开 pecl.php.net 所以需要挂代理
RUN pecl install $PECL_INSTALL_PACKGE

EXPOSE 9000
CMD ["php-fpm"]


#docker-php-ext-install pdo
#docker-php-ext-install pdo_mysql