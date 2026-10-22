# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.3-cli-alpine@sha256:25bcb4f1ce4f13651c0f89a6a2db2b14ac5a75555d3553fa03c83dff6d6966a8 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.27"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.3-cli-alpine@sha256:25bcb4f1ce4f13651c0f89a6a2db2b14ac5a75555d3553fa03c83dff6d6966a8

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
