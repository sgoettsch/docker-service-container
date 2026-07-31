# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.3-cli-alpine@sha256:730593ea1c4c2150e0a47c9c071633b955a21e1b167448f6f93b093fd2ff7f2d AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.17"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.3-cli-alpine@sha256:730593ea1c4c2150e0a47c9c071633b955a21e1b167448f6f93b093fd2ff7f2d

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
