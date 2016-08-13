# This dockerfile uses the php image
# Yii2 Test - 5.5
# Author: Francisco Barrento

FROM php:5.6
MAINTAINER Francisco Barrento francisco.barrento@gmail.com

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -yq apt-utils
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq htop

# Install mysql server
RUN echo mysql-server mysql-server/root_password password root | debconf-set-selections
RUN echo mysql-server mysql-server/root_password_again password root | debconf-set-selections
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq mysql-server

RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq \
    git \
    unzip \
    libpq5 \
    libfreetype6 \
    libjpeg62-turbo \
    libmagickwand-6.q16-2 \
    libmemcachedutil2 \
    libicu-dev \
    libpq-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmagickwand-dev \
    libmemcached-dev \
    --no-install-recommends \
    --fix-missing
RUN apt-get clean

RUN docker-php-ext-install intl
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-configure gd --enable-gd-native-ttf --with-freetype-dir=/usr/include/freetype2 --with-png-dir=/usr/include --with-jpeg-dir=/usr/include
RUN docker-php-ext-install gd
RUN pecl install imagick
RUN docker-php-ext-enable imagick
RUN pecl install memcached
RUN docker-php-ext-enable memcached
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

CMD ["php", "-a"]