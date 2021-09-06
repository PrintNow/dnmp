FROM centos:centos7 AS build

ENV NGINX_VERSION=1.21.0
ENV NGINX_URL=http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz

#安装相关依赖
RUN yum -y update && \
    yum -y install gcc gcc-c++ autoconf automake make && \
    yum -y install zlib zlib-devel openssl* pcre* wget

#MAINTAINER 维护者信息
MAINTAINER shine09 i@nowtime.cc

# 下载 Nginx 源码
ADD $NGINX_URL /tmp/

# 下载 nginx-http-concat 源码
ADD https://codeload.github.com/alibaba/nginx-http-concat/tar.gz/1.2.2 /tmp/

#切换目录
WORKDIR  /tmp

# 解压 http-concat 扩展
RUN tar -xzvf 1.2.2 && \
 cp -r nginx-http-concat-1.2.2/ /usr/local/src/ && \
 mkdir -p {/usr/local/nginx/logs,/var/lock} && \
 useradd -M -s /bin/bash nginx && \
 tar -zxvf nginx-$NGINX_VERSION.tar.gz && \
 mkdir -p /usr/local/nginx && \
 cd /tmp/nginx-$NGINX_VERSION \
    && ./configure --prefix=/etc/nginx --user=nginx --group=nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --with-http_stub_status_module \
    --with-http_ssl_module \
    --with-http_gzip_static_module \
    --with-http_gunzip_module \
    --with-ipv6 \
    --with-http_sub_module  \
    --with-http_flv_module  \
    --with-http_addition_module  \
    --with-http_realip_module  \
    --with-http_mp4_module  \
    --with-ld-opt="-Wl,-E"  \
    --with-cc-opt="-Wno-error" \
    --add-module=/usr/local/src/nginx-http-concat-1.2.2 \
    && make && make install && \
     rm -rf /tmp && \
    /etc/nginx/sbin/nginx -c /etc/nginx/nginx.conf && \
    ln -s /usr/local/nginx/sbin/* /usr/local/sbin/

#第二阶段
FROM centos:centos7
COPY --from=build /etc/nginx /etc/nginx
RUN mkdir -p {/var/lock,/var/log/nginx/} && \
    ln -s /etc/nginx/sbin/nginx /usr/local/sbin/nginx && \
    useradd -M -s/bin/bash nginx

#EXPOSE 映射端口
EXPOSE 80 443

#CMD 运行以下命令
#CMD ["nginx"]
CMD ["/etc/nginx/sbin/nginx","-g","daemon off;"]

#构建镜像
#docker build -t shine09/nginx:v1 .