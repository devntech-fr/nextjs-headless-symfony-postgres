FROM php:7.4-apache

ARG GIT_REPOSITORY_TO_CLONE
ARG DATABASE_URL

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

RUN apt-get update \
    && apt-get install -y --no-install-recommends locales apt-utils git wget iputils-ping nano libpq-dev libsodium-dev libicu-dev g++ libpng-dev libxml2-dev libzip-dev libonig-dev libxslt-dev;

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
   mv composer.phar /usr/local/bin/composer

RUN docker-php-ext-configure intl
RUN docker-php-ext-install pdo pdo_mysql pdo_pgsql pgsql sodium tokenizer gd opcache intl zip calendar dom mbstring zip gd xsl
RUN pecl install apcu && docker-php-ext-enable apcu
# Symfony CLI
RUN wget https://get.symfony.com/cli/installer -O - | bash && mv /root/.symfony/bin/symfony /usr/local/bin/symfony

RUN cd /var/www
RUN git clone ${GIT_REPOSITORY_TO_CLONE} symfony \
    && cd symfony && echo "DATABASE_URL='${DATABASE_URL}'" >> .env && composer install

WORKDIR /var/www/symfony
