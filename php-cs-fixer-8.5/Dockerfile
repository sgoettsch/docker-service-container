# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.5-cli-alpine@sha256:4992c6fda82eadfb3b22dca3929188dc5831e9f44a3d42b3c8d36a460b4d5a80 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.27"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.5-cli-alpine@sha256:4992c6fda82eadfb3b22dca3929188dc5831e9f44a3d42b3c8d36a460b4d5a80

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
