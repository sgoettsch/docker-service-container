# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.4-cli-alpine@sha256:37c8c64177612b72e7f9d2c00481739c166a3708bea27c1cfd27fea1291d82d1 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.27"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.4-cli-alpine@sha256:37c8c64177612b72e7f9d2c00481739c166a3708bea27c1cfd27fea1291d82d1

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
