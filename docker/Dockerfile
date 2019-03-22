FROM debian:stretch-slim

ENV NGINX_VERSION 1.15.9

RUN set -x \
	  && apt-get update \
	  && apt-get install --no-install-recommends --no-install-suggests -y \
       gnupg1 apt-transport-https ca-certificates \
       git wget build-essential \
       gettext \
       libpcre3 \
       libpcre3-dev \
       libxml2-dev \
       libxslt-dev \
       libgd-dev \
       libgeoip-dev \
       libssl-dev \
    && cd /tmp/ \
    && git clone https://github.com/nginx-modules/ngx_cache_purge.git \
    && wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
    && tar zxf nginx-${NGINX_VERSION}.tar.gz \
    && cd nginx-${NGINX_VERSION} \
    && ./configure \
        --prefix=/etc/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-client-body-temp-path=/var/lib/nginx/body \
        --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
        --http-log-path=/var/log/nginx/access.log \
        --http-proxy-temp-path=/var/lib/nginx/proxy \
        --http-scgi-temp-path=/var/lib/nginx/scgi \
        --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
        --lock-path=/var/lock/nginx.lock \
        --pid-path=/var/run/nginx.pid \
        --with-debug \
        --with-http_addition_module \
        --with-http_dav_module \
        --with-http_geoip_module \
        --with-http_gzip_static_module \
        --with-http_image_filter_module \
        --with-http_realip_module \
        --with-http_stub_status_module \
        --with-http_ssl_module \
        --with-http_sub_module \
        --with-http_xslt_module \
        --with-http_v2_module \
        --with-ipv6 \
        --with-sha1=/usr/include/openssl \
        --with-md5=/usr/include/openssl \
        --with-mail --with-mail_ssl_module \
        --add-module=/tmp/ngx_cache_purge \
    && make \
    && make install \
    && apt-get autoremove \
    && apt-get clean \
    && rm -fr /tmp/ngx_cache_purge \
    && rm -fr /tmp/nginx-${NGINX_VERSION}.tar.gz


# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	  && ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
