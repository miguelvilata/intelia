FROM php:7.4.0-fpm

ARG DEBUG=false
ARG REMOTE_HOST="127.0.0.1"

RUN apt-get update \
    &&  apt-get install -y --no-install-recommends \
        #supervisor \
        locales apt-utils git libicu-dev g++ libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev unzip libpq-dev nodejs npm wget \
        apt-transport-https lsb-release ca-certificates

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen  \
    &&  echo "es_ES.UTF-8 UTF-8" >> /etc/locale.gen \
    &&  locale-gen

RUN curl -sS https://getcomposer.org/installer | php -- \
    &&  mv composer.phar /usr/local/bin/composer

RUN curl -sS https://get.symfony.com/cli/installer | bash \
    &&  mv /root/.symfony5/bin/symfony /usr/local/bin

RUN docker-php-ext-configure \
            intl \
    &&  docker-php-ext-install \
            pdo pdo_mysql pdo_pgsql opcache intl zip calendar dom mbstring gd xsl

RUN pecl install apcu && docker-php-ext-enable apcu

RUN #npm install --global yarn

RUN git config --global user.email "miguel.vilata@gmail.com" \
    &&  git config --global user.name "Miguel Uve"

#CMD tail -f /dev/null

RUN apt-get install -y ffmpeg
RUN apt-get install -y vim
RUN apt-get install -y libimage-exiftool-perl

RUN docker-php-ext-install exif
RUN docker-php-ext-configure exif \
            --enable-exif

WORKDIR /var/www

# DEBUG
RUN if [ $DEBUG = "true" ] ; then \
    pecl install xdebug-2.6.0; \
    docker-php-ext-enable xdebug;  fi ;

RUN if [ $DEBUG = "true" ] ; then \
      cp /usr/local/etc/php/temporal_docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
      echo "\nxdebug.remote_host="$REMOTE_HOST >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi ;

COPY "./ini/php.ini-development" "/usr/local/etc/php/php.ini"
COPY "./ini/docker-php-ext-xdebug.ini" "/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini"
