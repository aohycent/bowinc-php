FROM ghcr.io/aohycent/xdebug:main

LABEL org.opencontainers.image.source="https://github.com/aohycent/bowinc-php" \
    maintainer="aohycent <hycentonline@gmail.com>"

ENV COMPOSER_PATH /usr/local/bin/composer

# Install extensions
RUN set -xe && \
        apt-get update -y && apt-get install -y --no-install-recommends --no-install-suggests \
            libgmp-dev \
            libjpeg-dev \
            libmemcached-dev \
            libpng-dev \
            libxml2-dev \
            libzip-dev \
            zlib1g-dev \
            unzip \
        && \
        apt-get clean && rm -rf /var/lib/apt/lists/* \
        && \
        # See https://github.com/docker-library/php/issues/912#issuecomment-559918036
        docker-php-ext-configure gd \
            --with-jpeg \
        && \
        docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) \
            bcmath \
            exif \
            gd \
            gmp \
            pdo_mysql \
            soap \
            sockets \
            zip \
        && \
        pecl install \
            memcached \
            mongodb \
            redis \
            swoole \
        && \
        docker-php-ext-enable \
            memcached \
            mongodb \
            redis \
            swoole \
        && \
        php -m

# Install Composer v2
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer
