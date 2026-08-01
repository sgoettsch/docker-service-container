# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.5-cli-alpine@sha256:8d7090ce03736b6ecfd739d87a46ab59b5d1d0d837f1ac7c5d2702f1d312f5a0 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.18"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.5-cli-alpine@sha256:8d7090ce03736b6ecfd739d87a46ab59b5d1d0d837f1ac7c5d2702f1d312f5a0

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
