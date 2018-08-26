FROM php:7.2-apache

# Setting locale
RUN apt-get update && \
    apt-get install -y locales && \
    rm -rf /var/lib/apt/lists/* && \
    echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen ja_JP.UTF-8
ENV LC_ALL ja_JP.UTF-8

# copy from custom bashrc
COPY .bashrc /root/

# install postgresql10 client
RUN apt-get update && apt-get install --no-install-recommends -y wget gnupg gnupg2 gnupg1
RUN wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | apt-key add -
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
RUN apt-get update && apt-get install --no-install-recommends -y postgresql-client-10

# install php middleware
RUN apt-get update && apt-get install --no-install-recommends -y \
        git curl unzip vim wget sudo libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libmcrypt-dev libxml2-dev libpq-dev libpq5 postgresql-client mysql-client ssl-cert libicu-dev \
        && docker-php-ext-configure \
        gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) \
        mbstring zip gd xml pdo pdo_pgsql pdo_mysql soap intl opcache \
        && rm -r /var/lib/apt/lists/*

# copy from custom php.ini file
COPY php.ini /usr/local/etc/php/

# setting apache virtualhost
COPY virtual.conf /etc/apache2/sites-available/
RUN mkdir -p /var/log/httpd/php7.localdomain
RUN mkdir -p /var/www/web

RUN a2enmod rewrite && a2dissite 000-default && a2ensite virtual && service apache2 restart

# install adminer
RUN mkdir -p /var/www/adminer \
  && cd /var/www/adminer \
  && wget https://www.adminer.org/static/download/4.6.3/adminer-4.6.3.php \
  && wget https://raw.githubusercontent.com/vrana/adminer/master/designs/nicu/adminer.css \
  && mv adminer-4.6.3.php index.php

# install composer and settings
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer \
        --install-dir=/usr/local/bin
RUN chown -R www-data: /var/www

USER www-data
RUN composer global require --optimize-autoloader \
        "hirak/prestissimo"
USER root

