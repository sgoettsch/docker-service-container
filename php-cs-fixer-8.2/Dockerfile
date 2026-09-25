# Builder stage – download the pinned php-cs-fixer phar
FROM php:8.2-cli-alpine@sha256:807e06766275f9d176ff6d773c32900317a0d256a59a4c23270026f5bbf56a3b AS builder

RUN apk add --no-cache curl

# renovate: datasource=github-releases depName=PHP-CS-Fixer/PHP-CS-Fixer
ENV PHP_CS_FIXER_VERSION="v3.95.25"
RUN curl -sS -L "https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/releases/download/${PHP_CS_FIXER_VERSION}/php-cs-fixer.phar" \
      -o /usr/bin/php-cs-fixer \
    && chmod +x /usr/bin/php-cs-fixer \
    && php /usr/bin/php-cs-fixer --version

# Final image
FROM php:8.2-cli-alpine@sha256:807e06766275f9d176ff6d773c32900317a0d256a59a4c23270026f5bbf56a3b

COPY --from=builder /usr/bin/php-cs-fixer /usr/bin/php-cs-fixer

WORKDIR /app
ENTRYPOINT ["php-cs-fixer"]
CMD ["--version"]
