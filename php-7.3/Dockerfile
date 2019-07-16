FROM php:7.3-fpm-buster

ARG PHP_REDIS_VERSION=5.0.1
ARG PHP_MONGODB_VERSION=1.5.3
ARG PHP_AMQP_VERSION=master
ARG PHP_PCOV_VERSION=1.0.6

RUN set -eux ; \
    docker-php-source extract ; \
    \
    cd /usr/src/php/ext ; \
    curl -fsSL https://github.com/igbinary/igbinary7/archive/master.tar.gz -o igbinary.tar.gz ; \
    mkdir -p igbinary ; \
    tar -xf igbinary.tar.gz -C igbinary --strip-components=1 ; \
    rm igbinary.tar.gz ; \
    \
    curl -fsSL https://github.com/phpredis/phpredis/archive/$PHP_REDIS_VERSION.tar.gz -o redis.tar.gz ; \
    mkdir -p redis ; \
    tar -xf redis.tar.gz -C redis --strip-components=1 ; \
    rm redis.tar.gz ; \
    \
    curl -fsSL https://pecl.php.net/get/mongodb-$PHP_MONGODB_VERSION.tgz -o mongodb.tgz ; \
    mkdir -p mongodb ; \
    tar -xf mongodb.tgz -C mongodb --strip-components=1 ; \
    rm mongodb.tgz ; \
    \
    curl -fsSL https://github.com/pdezwart/php-amqp/archive/$PHP_AMQP_VERSION.tar.gz -o amqp.tar.gz ; \
    mkdir -p amqp ; \
    tar -xf amqp.tar.gz -C amqp --strip-components=1 ; \
    rm amqp.tar.gz ; \
    \
    curl -fsSL https://github.com/krakjoe/pcov/archive/v$PHP_PCOV_VERSION.tar.gz -o pcov.tar.gz ; \
    mkdir -p pcov ; \
    tar -xf pcov.tar.gz -C pcov --strip-components=1 ; \
    rm pcov.tar.gz ; \
    \
    curl -fsSL https://pecl.php.net/get/imagick -o imagick.tgz ; \
    mkdir -p imagick ; \
    tar -xf imagick.tgz -C imagick --strip-components=1 ; \
    rm imagick.tgz ; \
    \
    echo "deb-src http://deb.debian.org/debian stretch main" >> /etc/apt/sources.list ; \
    apt-get update && apt-get install -y \
        ssh \
        gpg \
        dirmngr \
        libssl-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libsodium-dev \
        awscli \
        libicu-dev \
        git-core \
        unzip \
        locales \
        python \
        fontconfig \
        libgmp-dev \
        autotools-dev \
        automake \
        libtool \
        gpg \
        libpq-dev \
        librabbitmq-dev \
        libwebp-dev \
        devscripts \
        libgraphviz-dev \
        libmagickwand-6.q16-dev \
        libzip-dev \
    --no-install-recommends ; \
    docker-php-ext-install igbinary ; \
    docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ ; \
    docker-php-ext-configure redis --enable-redis-igbinary ; \
    docker-php-ext-install -j$(nproc) amqp iconv gd gmp intl imagick exif bcmath sockets mbstring mongodb pcov pdo pdo_mysql pdo_pgsql redis opcache zip ; \
    docker-php-source delete ; \
    \
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen ; \
    sed -i -e 's/# it_IT.UTF-8 UTF-8/it_IT.UTF-8 UTF-8/' /etc/locale.gen ; \
    locale-gen ; \
    apt-mark auto \
        libssl-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libsodium-dev \
        awscli \
        libicu-dev \
        git-core \
        unzip \
        python \
        fontconfig \
        libgmp-dev \
        autotools-dev \
        automake \
        libtool \
        gpg \
        libpq-dev \
        librabbitmq-dev \
        libwebp-dev \
        devscripts \
        libgraphviz-dev \
        libzip-dev \
        libmagickwand-6.q16-dev \
    apt-get autoremove -y ; \
    apt-get install -y \
        libpq5 \
        libzip4 \
        librabbitmq4 \
        libmagickwand-6.q16-6 \
        libpng16-16 \
        unzip \
        --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN echo "memory_limit=512M" >> /usr/local/etc/php/php.ini
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD .build /.build
RUN /.build/install-node

