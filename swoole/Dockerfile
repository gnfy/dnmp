FROM php:7.4.25-fpm-alpine

MAINTAINER gnfy

RUN \
    curl -sfL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
    chmod +x /usr/bin/composer                                                                     && \
    composer self-update --clean-backups 2.1.9                                    && \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
    apk update && \
    apk add --no-cache libstdc++ && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS curl-dev openssl-dev pcre-dev pcre2-dev zlib-dev libzip-dev && \
    apk add --no-cache libpng-dev libwebp-dev freetype-dev libjpeg-turbo-dev && \
    docker-php-ext-install sockets && \
    docker-php-source extract && \
    # 下载解压swoole
    mkdir /usr/src/php/ext/swoole && \
    # curl -sfL https://github.com/swoole/swoole-src/archive/master.tar.gz -o swoole.tar.gz && \
    curl -sfL https://github.com/swoole/swoole-src/archive/refs/tags/v4.8.1.tar.gz -o swoole.tar.gz && \
    tar xfz swoole.tar.gz --strip-components=1 -C /usr/src/php/ext/swoole && \
    docker-php-ext-configure swoole \
        --enable-http2   \
        --enable-mysqlnd \
        --enable-openssl \
        --enable-sockets --enable-swoole-curl --enable-swoole-json && \
    docker-php-ext-install swoole && \
    echo "swoole.use_shortname = 'Off'" >> /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini && \
    # 下载解压redis
    mkdir /usr/src/php/ext/redis && \
    curl -sfL https://github.com/phpredis/phpredis/archive/refs/tags/5.3.4.tar.gz -o php-redis.tar.gz && \
    tar xfz php-redis.tar.gz --strip-components=1 -C /usr/src/php/ext/redis && \
    # 下载解压mongodb
    mkdir /usr/src/php/ext/mongodb && \
    curl -sfL http://pecl.php.net/get/mongodb-1.11.1.tgz -o php-mongodb.tgz && \
    tar xfz php-mongodb.tgz --strip-components=1 -C /usr/src/php/ext/mongodb && \
    # 安装其它的扩展
    docker-php-ext-install bcmath pcntl opcache gd mysqli pdo_mysql redis mongodb && \
    echo "opcache.enable_cli = 'On'" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini && \
    rm -f swoole.tar.gz $HOME/.composer/*-old.phar && \
    rm -f php-redis.tar.gz php-mongodb && \
    docker-php-source delete && \
    apk del .build-deps

WORKDIR "/var/www/"
