FROM php:7.4.0-fpm

ARG DEBUG=false

RUN apt-get update \
    &&  apt-get install -y --no-install-recommends \
        #supervisor \
        locales apt-utils git libicu-dev g++ libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev unzip libpq-dev nodejs npm wget \
        apt-transport-https lsb-release ca-certificates

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen  \
    &&  echo "es_ES.UTF-8 UTF-8" >> /etc/locale.gen \
    &&  locale-gen

RUN docker-php-ext-configure \
            intl \
    &&  docker-php-ext-install \
            pdo pdo_mysql pdo_pgsql opcache intl zip calendar dom mbstring gd xsl

RUN pecl install apcu && docker-php-ext-enable apcu

RUN apt-get install -y vim

RUN docker-php-ext-install exif
RUN docker-php-ext-configure exif \
            --enable-exif

WORKDIR /var/www

COPY "./ini/php.ini-development" "/usr/local/etc/php/php.ini"
COPY "./ini/docker-php-ext-xdebug.ini" "/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini"
COPY "./scripts/message-consumer.sh" "/bin/message-consumer.sh"


ENTRYPOINT ["bash", "/bin/message-consumer.sh"]

