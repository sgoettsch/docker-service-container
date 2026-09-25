# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.3-cli-alpine@sha256:3c61b92c1c3c024fe6ff4022b5ef8814b9beead8c74e8a5344318b5f43543a47 AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.25"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.3-cli-alpine@sha256:3c61b92c1c3c024fe6ff4022b5ef8814b9beead8c74e8a5344318b5f43543a47

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
