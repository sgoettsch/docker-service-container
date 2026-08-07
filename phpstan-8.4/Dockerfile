FROM ghcr.io/php/pie:bin@sha256:9fb4fb481767c48f7c57a7fc2c1ccef6ec8fc1e254751a77213cfb0c8f181eba AS pie-installer

### ------------------------------------------------
### Builder stage – OpenSwoole
### ------------------------------------------------
FROM ghcr.io/phpstan/phpstan:latest-php8.4@sha256:e4a8e95aaf238d888c8329a998e1144c8f597c16177d69a8eaf96d903089b0b6 AS openswoole-builder

# Openswoole
# renovate: datasource=github-tags depName=openswoole/swoole-src versioning=semver
ENV OPENSWOOLE_VERSION="v26.2.0"
RUN apk add --no-cache linux-headers && \
    apk add --no-cache libstdc++ postgresql-dev libpq liburing liburing-dev && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS curl-dev openssl-dev pcre-dev pcre2-dev zlib-dev && \
    docker-php-ext-install sockets && \
    docker-php-source extract && \
    cd /tmp && git clone https://github.com/openswoole/ext-openswoole.git
WORKDIR /tmp/ext-openswoole
RUN git checkout ${OPENSWOOLE_VERSION} && \
    phpize && \
    ./configure --enable-openssl --enable-hook-curl --enable-http2 --enable-mysqlnd && \
    make && \
    make install && \
    strip /usr/local/lib/php/extensions/*/openswoole.so

### ------------------------------------------------
### Final image
### ------------------------------------------------
FROM ghcr.io/phpstan/phpstan:latest-php8.4@sha256:e4a8e95aaf238d888c8329a998e1144c8f597c16177d69a8eaf96d903089b0b6

RUN apk add --no-cache icu-dev libxml2-dev libzip-dev linux-headers && \
    docker-php-ext-install intl soap mysqli sockets xml zip

# Composer
# renovate: datasource=github-tags depName=composer/composer versioning=semver
ENV COMPOSER_VERSION="2.10.2"
RUN curl -sfL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
    chmod +x /usr/bin/composer && \
    composer self-update --clean-backups ${COMPOSER_VERSION}

### OpenSwoole
COPY --from=openswoole-builder /usr/local/lib/php/extensions/ /usr/local/lib/php/extensions/
RUN touch /usr/local/etc/php/conf.d/openswoole.ini && \
    echo 'extension=openswoole.so' > /usr/local/etc/php/conf.d/zzz_openswoole.ini

# Redis
# renovate: datasource=github-tags depName=phpredis/phpredis versioning=semver
ENV PHP_REDIS_VERSION="6.3.0"
RUN --mount=type=bind,from=pie-installer,source=/pie,target=/usr/local/bin/pie \
    apk add --no-cache pcre-dev $PHPIZE_DEPS && \
    pie install phpredis/phpredis:${PHP_REDIS_VERSION} && \
    docker-php-ext-enable redis.so

WORKDIR /app
