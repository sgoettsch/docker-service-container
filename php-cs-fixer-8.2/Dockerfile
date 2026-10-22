# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.2-cli-alpine@sha256:8854cc5e636820f38267855e97d0825659bc6acaf0891aa9701ec7e5c9ecc165 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.27"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.2-cli-alpine@sha256:8854cc5e636820f38267855e97d0825659bc6acaf0891aa9701ec7e5c9ecc165

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
