# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.3-cli-alpine@sha256:e7699f91d3b46283a49bde02379266dd98d87844e08aed1ff7ea6a7d1f031268 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.30"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.3-cli-alpine@sha256:e7699f91d3b46283a49bde02379266dd98d87844e08aed1ff7ea6a7d1f031268

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
