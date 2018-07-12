FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
        gpg \
        locales \
        git-core \
        librabbitmq-dev \
        libsodium-dev \
        libssl-dev \
        libicu-dev \
        zlib1g-dev \
        python \
        unzip \
    &&  docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql iconv intl mbstring opcache zip

RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    sed -i -e 's/# it_IT.UTF-8 UTF-8/it_IT.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen

RUN pecl install amqp && docker-php-ext-enable amqp
RUN pecl install mongodb && docker-php-ext-enable mongodb

ADD .config/.ssh /root/.ssh
RUN chmod 600 ~/.ssh/id_rsa
RUN ssh-keyscan -t rsa codebasehq.com > ~/.ssh/known_hosts && \
    echo "    IdentityFile ~/.ssh/id_rsa" >> /etc/ssh/ssh_config

RUN echo "memory_limit=512M" >> /usr/local/etc/php/php.ini

RUN cp .env.dist .env
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install --no-progress

RUN YARN_VERSION=1.7.0 bash .build/install-node
RUN yarn