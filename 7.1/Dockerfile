# This dockerfile uses the php image
# Laravel Test - 7.1
# Author: Francisco Barrento

FROM php:7.1
MAINTAINER Francisco Barrento francisco.barrento@gmail.com

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -yq apt-utils
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq htop

# Install mysql server
RUN echo mysql-server mysql-server/root_password password root | debconf-set-selections
RUN echo mysql-server mysql-server/root_password_again password root | debconf-set-selections
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq mysql-server

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    apt-get install -yq \
    apt-transport-https \
    ca-certificates \
    curl \
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

ADD https://packages.sury.org/php/apt.gpg /etc/apt/trusted.gpg.d/php.gpg
RUN echo "deb https://packages.sury.org/php/ jessie main" > /etc/apt/sources.list.d/php.list

RUN apt-get update && \
    apt-get install -y \
    php7.1-fpm \
    php7.1-cli \
    php7.1-mbstring \
    php7.1-xml \
    php7.1-zip \
    php7.1-curl \
    php7.1-intl \
    php7.1-gd \
    php7.1-sqlite3 \
    php7.1-mysql \
    php7.1-pgsql \
    rubygems

RUN php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

# Is this good?
RUN php -v && \
    git --version && \
    composer --version

RUN docker-php-ext-install intl
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-configure gd --enable-gd-native-ttf --with-freetype-dir=/usr/include/freetype2 --with-png-dir=/usr/include --with-jpeg-dir=/usr/include
RUN docker-php-ext-install gd
RUN pecl install imagick
RUN docker-php-ext-enable imagick
RUN set -xe cd /tmp \
&& curl -sO https://codeload.github.com/php-memcached-dev/php-memcached/zip/php7 \
&& unzip php7 \
&& cd php-memcached-php7 \
&& phpize \
&& ./configure \
&& make install \
&& cd / && rm -rf /tmp/php-memcached-php7 \
&& docker-php-ext-enable memcached

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

VOLUME /var/lib/mysql

CMD ["php", "-a"]