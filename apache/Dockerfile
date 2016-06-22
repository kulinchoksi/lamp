FROM php:7.0.3-apache

LABEL Description = "This image is used to run PHP 7, Apache and some required utilities with PHP extensions"
MAINTAINER Kulin Choksi "kulinchoksi@gmail.com"

ENV APACHE_RUN_USER=www-data
ENV APACHE_RUN_GROUP=www-data
ENV APACHE_PID_FILE=/var/run/apache2.pid
ENV APACHE_RUN_DIR=/var/run/apache2
ENV APACHE_LOCK_DIR=/var/lock/apache2
ENV APACHE_LOG_DIR=/var/log/apache2
ENV APACHE_USER_UID=0

COPY php/php.ini /usr/local/etc/php/
COPY www/ /var/www/html/
COPY logs/ /var/logs/
COPY apache/sites-available/* /etc/apache2/sites-available/

RUN apt-get update -qq \
    && DEBIAN_FRONTEND=noninteractive apt-get install -yqq \
        libreadline-gplv2-dev \
        libfreetype6 \
        apt-utils \
        dialog \
        debconf-utils \
        tzdata \
        unzip \
        wget \
        curl \
        vim \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo_mysql zip xdebug \
    && apt-get purge --auto-remove -y zlib1g-dev \
    && apt-get clean \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && echo 'alias ll="ls -lah --color=auto"' >> /etc/bash.bashrc \
    && echo 'EXPORT COMPOSER_PROCESS_TIMEOUT=900' >> /etc/profile \
    && echo 'EXPORT PATH=$PATH:/root/.composer/vendor/bin/' >> /etc/profile \
    && a2enmod rewrite && a2ensite root playground && service apache2 restart

COPY docker-library/php/7.0/apache/docker-php-ext-* /usr/local/bin/

