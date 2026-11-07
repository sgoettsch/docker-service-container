# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.2-cli-alpine@sha256:212a8b65f28529edcff248b9140edfac3ed8d651917e0eedb1fe6d3fa6950493 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.29"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.2-cli-alpine@sha256:212a8b65f28529edcff248b9140edfac3ed8d651917e0eedb1fe6d3fa6950493

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
