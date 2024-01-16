FROM php:8.1-apache-bullseye
#FROM php:8.1-apache

ENV OHRM_VERSION 5.5
ENV OHRM_MD5 113e76fa9dd42a03f2b6a397fa2ffbc8

USER root
RUN apt-get update && \
  apt-get install --yes --force-yes \
  cron g++ gettext libicu-dev openssl \
  libc-client-dev libmcrypt-dev \
  libtidy-dev libcurl4-openssl-dev \
  libz-dev libmemcached-dev git-core libpq-dev \
  libzip4 libzip-dev libwebp-dev

RUN docker-php-ext-install  exif gettext iconv intl  tidy zip sockets&&\
  docker-php-ext-install dba mysqli pdo_mysql &&\
  pecl install xdebug && docker-php-ext-enable xdebug &&\
  echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini &&\
  echo "xdebug.client_host = host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini &&\
  pecl install redis && docker-php-ext-enable redis && \
  curl -sS https://getcomposer.org/installer | php \
  && mv composer.phar /usr/bin/composer
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

WORKDIR /var/www/html
COPY . .
RUN chown -R  www-data:www-data /var/www/html
RUN chmod -R 775 /var/www/html

RUN a2enmod rewrite setenvif

CMD ["apache2-foreground"]