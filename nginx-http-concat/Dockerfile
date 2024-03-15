FROM ubuntu:latest

# 更新系统源为阿里云
RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list

# 安装构建 Nginx 和下载模块所需的工具
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    libpcre3 \
    libpcre3-dev \
    libssl-dev \
    zlib1g-dev \
    git \
    curl \
    ca-certificates \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 添加 nginx 用户和组
RUN groupadd -r nginx && useradd -r -g nginx nginx

# 设置工作目录
WORKDIR /usr/src

# 下载并解压 Nginx 源码
RUN curl -LO https://nginx.org/download/nginx-1.25.4.tar.gz && \
    tar -xzvf nginx-1.25.4.tar.gz && \
    rm nginx-1.25.4.tar.gz

# 下载并解压 nginx-http-concat 模块源码
RUN git clone https://github.com/alibaba/nginx-http-concat.git

# 构建 Nginx 并添加 nginx-http-concat 模块
RUN cd nginx-1.25.4 && \
    ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-perl_modules_path=/usr/lib/perl5/vendor_perl --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-Os -fomit-frame-pointer -g' --with-ld-opt=-Wl,--as-needed,-O1,--sort-common --add-module=../nginx-http-concat && \
    make && \
    make install

# 创建缓存目录
RUN mkdir -p /var/cache/nginx/client_temp /var/cache/nginx/proxy_temp /var/cache/nginx/fastcgi_temp /var/cache/nginx/uwsgi_temp /var/cache/nginx/scgi_temp

# 清理工作目录和安装的工具
RUN rm -rf /usr/src/nginx-1.25.4 && \
    rm -rf /usr/src/nginx-http-concat

# 设置 Nginx 配置文件路径
# ENV PATH="/usr/local/nginx/sbin:${PATH}"

# 更改日志文件权限
RUN chown -R nginx:nginx /var/log/nginx

# 启动 Nginx
CMD ["nginx", "-g", "daemon off;"]
