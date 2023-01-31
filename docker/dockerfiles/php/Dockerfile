FROM php:8.1-fpm

ARG PHP_USER_ID
ARG PHP_GROUP_ID

RUN apt-get update && apt-get install -y \
curl \
openssl \
git \
gnupg2 \
libicu-dev \
libxml2-dev \
libxslt-dev \
libfreetype6-dev \
libjpeg62-turbo-dev \
libpng-dev \
libgd-dev \
libpq-dev \
libonig-dev \
libbz2-dev \
zlib1g-dev \
libzip-dev

#other
RUN pecl install redis xdebug-3.1.2 \
    && docker-php-ext-enable redis.so \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-install opcache

COPY ./config/php/docker-php-ext-opcache.ini /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

COPY ./config/php/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure intl \
    && docker-php-ext-configure pgsql

RUN docker-php-ext-install -j$(nproc) intl \
    dom xsl \
    gd exif \
    mysqli pdo pdo_mysql pdo_pgsql pgsql \
    gettext mbstring \ 
    zip bz2

RUN usermod -u ${PHP_USER_ID} www-data && groupmod -g ${PHP_GROUP_ID} www-data

USER "${PHP_USER_ID}:${PHP_GROUP_ID}"

CMD ["php-fpm"]