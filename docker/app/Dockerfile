FROM php:5.6.31-apache

COPY ./docker/app/php.ini /usr/local/etc/php/

# Counterplan for deprecated Debian Jessie
RUN echo "deb http://archive.debian.org/debian/ stretch main contrib non-free" > /etc/apt/sources.list && \
    echo "deb-src http://archive.debian.org/debian/ stretch main contrib non-free" >> /etc/apt/sources.list && \
    echo "Acquire::Check-Valid-Until false;" > /etc/apt/apt.conf

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    gnupg \
    ca-certificates \
    curl \
    git \
    vim \
    zip \
    unzip \
    libzip-dev \
    libssl-dev \
    libpq-dev
RUN docker-php-ext-install mbstring pdo pdo_pgsql pgsql

COPY ./docker/app/apache2/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY ./docker/app/apache2/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf
COPY ./docker/app/apache2/fqdn.conf /etc/apache2/conf-available/fqdn.conf

RUN a2enconf fqdn \
    && a2enmod rewrite \
    && a2ensite default-ssl \
    && a2enmod mime

WORKDIR /var/www/html

RUN sed -i 's/Listen 80/Listen 80/' /etc/apache2/ports.conf


# or try with below...;
# https://qiita.com/imaiworks/items/cabb9e21cfbc75a838f8
# FROM moodlehq/moodle-php-apache:5.6

# WORKDIR /www

# RUN apt-get -y update
# RUN apt-get install -y init
# RUN apt-get install -y libxml2-dev
# RUN apt-get install -y libpng-dev
# RUN apt-get -y install cronolog
# RUN apt-get install -y default-mysql-client

# RUN a2enmod rewrite
# RUN a2ensite default-ssl 
# RUN a2enmod ssl
# RUN apt -y install certbot 

# RUN docker-php-ext-install pdo pdo_mysql mbstring xml mysql
# RUN docker-php-ext-install -j$(nproc) gd

# RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
# RUN php composer-setup.php
# RUN php -r "unlink('composer-setup.php');"
# RUN cp composer.phar /usr/bin/composer ; chmod 777 /usr/bin/composer


# CMD /usr/sbin/apache2ctl -DFOREGROUND
