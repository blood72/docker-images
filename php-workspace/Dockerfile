FROM php:8.0-alpine

RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        libtool \
        libxml2-dev \
        openssl-dev \
    && apk add --no-cache \
        git \
        curl

RUN apk add --no-cache --virtual .zip-build-deps \
        libzip-dev \
    && apk add --no-cache \
        libzip \
    && docker-php-ext-install \
        zip \
    && apk del -f .zip-build-deps

RUN apk add --no-cache --virtual .intl-build-deps \
        icu-dev \
    && apk add --no-cache \
        icu \
        libintl \
    && docker-php-ext-install \
        intl \
    && apk del -f .intl-build-deps

RUN docker-php-ext-install \
    bcmath \
    dom \
    fileinfo \
    filter \
    iconv \
    opcache \
    pdo \
    phar \
    pcntl \
    sockets \
    tokenizer \
    xml \
    xmlwriter

RUN apk add --no-cache imap-dev \
    && docker-php-ext-configure imap --with-imap-ssl \
    && docker-php-ext-install imap

RUN apk add --no-cache --virtual .sqlite-build-deps \
        sqlite-dev \
    && docker-php-ext-install \
        pdo_sqlite \
    && apk del -f .sqlite-build-deps

RUN apk add --no-cache --virtual .mysql-build-deps \
        mysql-client \
        mariadb-client \
    && docker-php-ext-install \
        pdo_mysql \
    && apk del -f .mysql-build-deps

RUN apk add --no-cache --virtual .pgsql-build-deps \
        postgresql-client \
        postgresql-dev \
    && apk add --no-cache \
        libpq \
    && docker-php-ext-install \
        pdo_pgsql \
    && apk del -f .pgsql-build-deps

RUN apk add --no-cache --virtual .gd-build-deps \
        freetype-dev \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        libxpm-dev \
    && apk add --no-cache \
        freetype \
        libjpeg-turbo \
        libwebp \
        libpng \
        libxpm \
    && docker-php-ext-configure gd \
        --with-freetype=/usr/include/ \
        --with-xpm=/usr/include/ \
        --with-webp=/usr/include/ \
        --with-jpeg=/usr/include/ \
    && docker-php-ext-install gd \
    && apk del -f .gd-build-deps

RUN apk add --no-cache imagemagick imagemagick-dev \
    && git clone https://github.com/Imagick/imagick && cd imagick \
    && git checkout master && git pull \
    && phpize && ./configure && make && make install \
    && cd .. && rm -rf imagick \
    && docker-php-ext-enable imagick \
    && apk del imagemagick-dev

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN rm -rf /tmp/pear \
    && apk del -f .build-deps

RUN apk add --no-cache ffmpeg

RUN curl -sS https://getcomposer.org/installer | php; mv composer.phar /usr/local/bin/composer;
RUN composer global require laravel/installer
ENV PATH="/root/.composer/vendor/bin:${PATH}"

RUN apk add --update nodejs
RUN apk add --update npm

WORKDIR /var/www/html
