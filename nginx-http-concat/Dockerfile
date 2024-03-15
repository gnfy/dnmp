FROM alpine:latest

# 设置 Alpine Linux 的软件源为阿里云
RUN echo "https://mirrors.aliyun.com/alpine/latest-stable/main" > /etc/apk/repositories \
    && echo "https://mirrors.aliyun.com/alpine/latest-stable/community" >> /etc/apk/repositories

# 更新软件包索引并安装构建 Nginx 和下载模块所需的工具
RUN apk update \
    && apk add --no-cache \
        gcc \
        libc-dev \
        make \
        openssl-dev \
        pcre-dev \
        zlib-dev \
        git

# 设置工作目录
WORKDIR /usr/src

# 下载并解压 Nginx 源码
RUN wget https://nginx.org/download/nginx-1.25.4.tar.gz \
    && tar -xzvf nginx-1.25.4.tar.gz \
    && rm nginx-1.25.4.tar.gz

# 下载并解压 nginx-http-concat 模块源码
RUN git clone https://github.com/alibaba/nginx-http-concat.git

# 构建 Nginx 并添加 nginx-http-concat 模块
RUN cd nginx-1.25.4 \
    && ./configure --add-module=../nginx-http-concat \
    && make \
    && make install

# 清理工作目录和安装的工具
RUN rm -rf /usr/src/nginx-1.25.4 \
    && rm -rf /usr/src/nginx-http-concat \
    && apk del git gcc libc-dev make openssl-dev pcre-dev zlib-dev

# 启动 Nginx
CMD ["nginx", "-g", "daemon off;"]
