FROM php:7.2-fpm

RUN set -eux ; \
    apt-get update && apt-get install -y \
        ssh \
        gpg \
        dirmngr \
        locales \
        git-core \
        librabbitmq-dev \
        libsodium-dev \
        libssl-dev \
        libicu-dev \
        zlib1g-dev \
        python \
        unzip \
    --no-install-recommends && rm -r /var/lib/apt/lists/* ; \
    docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql iconv intl pcntl mbstring opcache zip ; \
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen ; \
    sed -i -e 's/# it_IT.UTF-8 UTF-8/it_IT.UTF-8 UTF-8/' /etc/locale.gen ; \
    locale-gen

RUN set -eux ; \ 
    pecl install amqp && docker-php-ext-enable amqp ; \
    pecl install mongodb && docker-php-ext-enable mongodb

RUN echo "memory_limit=512M" >> /usr/local/etc/php/php.ini
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD .build /.build
RUN YARN_VERSION=1.7.0 /.build/install-node

