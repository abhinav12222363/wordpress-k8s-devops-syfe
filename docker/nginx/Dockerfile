FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    build-essential \
    libpcre3 libpcre3-dev \
    libssl-dev \
    zlib1g zlib1g-dev \
    curl \
    wget \
    ca-certificates \
    perl \
    make \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/src

RUN wget https://openresty.org/download/openresty-1.25.3.1.tar.gz \
    && tar -xzf openresty-1.25.3.1.tar.gz

WORKDIR /usr/local/src/openresty-1.25.3.1

RUN ./configure \
    --with-http_ssl_module \
    --with-http_stub_status_module \
    --with-http_realip_module \
    --with-http_gzip_static_module \
    --with-pcre-jit \
    --with-luajit \
    && make -j$(nproc) \
    && make install

ENV PATH="/usr/local/openresty/bin:/usr/local/openresty/nginx/sbin:$PATH"

EXPOSE 80

CMD ["/usr/local/openresty/nginx/sbin/nginx", "-g", "daemon off;"]

