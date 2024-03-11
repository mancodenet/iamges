# Nginx 服务镜像
# sudo docker build --build-arg VERSION=1.0.0 -t nginx-ubuntu:1.0.0 .
# sudo docker run -d -p 80:80 nginx-ubuntu:1.0.0

FROM ubuntu:22.04

WORKDIR /opt/app

LABEL maintainer="mancode@sohu.com"

# 定义环境变量
ENV NGINX_PACKAGE_PATH /opt/app/nginx
ENV NGINX_PATH /usr/local/nginx

COPY ./nginx/nginx-1.25.4.tar.gz ${NGINX_PACKAGE_PATH}/nginx-1.25.4.tar.gz

COPY ./nginx/docs/index.html ${NGINX_PATH}/www/index.html

RUN apt update && apt install -y gcc g++ make libssl-dev libpcre3 libpcre3-dev zlib1g-dev \
  && useradd -M -s /sbin/nologin nginx \
  && cd ${NGINX_PACKAGE_PATH} && tar -zxf nginx-1.25.4.tar.gz && cd nginx-1.25.4 \ 
  && ./configure \
  --prefix=/usr/local/nginx \
  --conf-path=/etc/nginx/nginx.conf \
  --http-client-body-temp-path=/var/cache/nginx/client_temp \
  --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
  --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
  --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
  --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
  --with-http_stub_status_module \
  --with-http_ssl_module \
  --with-http_v2_module \
  --with-http_gzip_static_module \
  --with-http_sub_module \
  && make && make install \
  && mkdir -p /var/log/nginx \
  && mkdir -p /var/cache/nginx

COPY ./nginx/conf/ /etc/nginx/

ENTRYPOINT ["/usr/local/nginx/sbin/nginx", "-g", "daemon off; master_process on;"]
