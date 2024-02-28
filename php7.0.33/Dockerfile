FROM php:7.0.33-fpm-alpine

MAINTAINER gnfy

RUN \
    curl -sfL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
    chmod +x /usr/bin/composer                                                                     && \
    composer self-update --clean-backups 2.1.9                                    && \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
    apk update && \
    apk add --no-cache libstdc++ && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS curl-dev openssl-dev pcre-dev pcre2-dev zlib-dev libzip-dev && \
    apk add --no-cache libpng-dev libwebp-dev freetype-dev libjpeg-turbo-dev libxml2-dev libmcrypt-dev bzip2-dev gettext-dev gmp-dev libxslt-dev icu-dev librdkafka-dev && \
    docker-php-ext-install sockets && \
    docker-php-source extract && \
    # 下载解压redis
    mkdir /usr/src/php/ext/redis && \
    curl -sfL https://github.com/phpredis/phpredis/archive/refs/tags/5.3.7.tar.gz -o php-redis.tar.gz && \
    tar xfz php-redis.tar.gz --strip-components=1 -C /usr/src/php/ext/redis && \
    # 下载解压mongodb
    # mkdir /usr/src/php/ext/mongodb && \
    # curl -sfL https://pecl.php.net/get/mongodb-1.11.1.tgz -o php-mongodb.tgz && \
    # tar xfz php-mongodb.tgz --strip-components=1 -C /usr/src/php/ext/mongodb && \
    # 下载解压kafka
    mkdir /usr/src/php/ext/rdkafka && \
    curl -sfL https://github.com/arnaud-lb/php-rdkafka/archive/refs/tags/6.0.3.tar.gz -o php-rdkafka.tar.gz && \
    tar xfz php-rdkafka.tar.gz  --strip-components=1 -C /usr/src/php/ext/rdkafka && \
    # 安装其它的扩展
    docker-php-ext-install bcmath pcntl opcache gd mysqli pdo_mysql redis calendar soap dom exif bz2 mcrypt gettext dba gmp shmop sysvmsg sysvsem sysvshm xsl wddx zip intl rdkafka && \
    echo "opcache.enable_cli = 'On'" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini && \
    rm -f swoole.tar.gz $HOME/.composer/*-old.phar && \
    rm -f php-redis.tar.gz php-mongodb && \
    docker-php-source delete && \
    apk del .build-deps

WORKDIR "/var/www/"
