FROM php:8.4-cli

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libpq-dev \
        libcurl4-openssl-dev \
        libpq5 \
    && docker-php-ext-install \
        pdo_pgsql \
        curl \
    && apt-get purge -y --auto-remove \
        libpq-dev \
        libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY router.php .

EXPOSE 8087

CMD ["php", "-S", "0.0.0.0:8087", "router.php"]