FROM php:7.4-fpm-alpine

# Install tools required for build stage
RUN apk add --update --no-cache \
    bash curl wget rsync ca-certificates openssl openssh git tzdata openntpd \
    libxrender fontconfig libc6-compat \
    mysql-client gnupg binutils-gold autoconf \
    g++ gcc gnupg libgcc linux-headers make python

# INstall composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
  && chmod 755 /usr/bin/composer

# Install additional php libraries
RUN docker-php-ext-install bcmath pdo_mysql

# Install libraries for compiling GD, then build it
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
  && docker-php-ext-install gd \
      && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

# add ZIP archives support
RUN apk add --update --no-cache zlib-dev libzip-dev \
  && docker-php-ext-install zip


RUN pecl install xdebug-2.7.2 \
  && docker-php-ext-enable xdebug

# Enabl XDebug
ADD xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /app