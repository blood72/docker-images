FROM php:8.0-alpine

RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        libtool \
        libxml2-dev \
        openssl-dev \
        sqlite-dev \
    && apk add --no-cache \
        git \
        mysql-client \
        mariadb-client \
        libintl \
        icu \
        icu-dev \
        libzip-dev \
        curl

RUN docker-php-ext-install \
    bcmath \
    dom \
    fileinfo \
    filter \
    iconv \
    intl \
    opcache \
    pdo \
    pdo_mysql \
    pdo_sqlite \
    phar \
    pcntl \
    tokenizer \
    xml \
    xmlwriter \
    zip

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

RUN apk add --no-cache ffmpeg

RUN apk add --no-cache imap-dev \
    && docker-php-ext-configure imap --with-imap-ssl \
    && docker-php-ext-install imap

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN rm -rf /tmp/pear \
    && apk del -f .build-deps

RUN curl -sS https://getcomposer.org/installer | php; mv composer.phar /usr/local/bin/composer;
RUN composer global require laravel/installer
ENV PATH="/root/.composer/vendor/bin:${PATH}"

RUN apk add --update nodejs
RUN apk add --update npm

WORKDIR /var/www/html
