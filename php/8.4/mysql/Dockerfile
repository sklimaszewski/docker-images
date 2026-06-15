# Build arguments
ARG VERSION=8.4

# Alpine base image
FROM php:${VERSION}-fpm-alpine

# Common dependencies
RUN wget https://getcomposer.org/installer && \
    php installer --install-dir=/usr/local/bin/ --filename=composer && \
    rm installer

RUN apk update && \
    apk add --no-cache --virtual dev-deps autoconf gcc g++ make && \
    apk add --no-cache git rsync bash bash-completion nano curl unzip openssl-dev shadow \
        libzip-dev libxslt-dev icu-dev freetype-dev libxpm-dev libgomp \
        libpng-dev libjpeg-turbo-dev libwebp-dev libwebp-tools \
        imagemagick-dev imagemagick jpegoptim pngquant \
        nodejs yarn && \
    echo "no" | pecl install redis

# MySQL dependencies
RUN apk add --no-cache mysql-client && \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-enable opcache redis && \
    docker-php-ext-install mysqli pdo_mysql exif gd pcntl intl xsl zip

# Imagick
RUN pecl install imagick-3.8.0 \
    && docker-php-ext-enable imagick

# Cleanup
RUN apk del --no-cache dev-deps && \
    rm -rf /var/cache/apk/* && rm -rf /tmp/*

# Final touch
ENV PS1="\u@\h:\w\\$ "

RUN mkdir -p /var/www/html/project/symfony
WORKDIR /var/www/html/project/symfony

CMD ["php-fpm"]