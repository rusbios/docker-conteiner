FROM debian

ARG PHP_VERSION=7.4

COPY scripts/autoclean.sh /root/
COPY scripts/docker-entrypoint.sh ./misc/cronfile.final ./misc/cronfile.system /

RUN echo $PHP_VERSION > /PHP_VERSION; \
    chmod +x /root/autoclean.sh; \
    chmod +x /docker-entrypoint.sh; \
    mkdir /app; \
    mkdir /run/php/; \
    mkdir -p /app; \
    apt-get update;

RUN apt-get install -y nginx;

RUN apt-get update; \
    PHP_EXTENSIONS="common curl intl mbstring zip bcmath fpm mysql xml"; \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends --no-install-suggests \
    readline-common less vim git \
    cron curl \
    php${PHP_VERSION} $(for p in $(echo ${PHP_EXTENSIONS}); do echo php${PHP_VERSION}-${p}; done) \
    unzip; \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends --no-install-suggests \
    composer phpunit

COPY php/www.conf /etc/php/$PHP_VERSION/fpm/pool.d/
COPY php/php-fpm.conf ./php/php.ini /etc/php/$PHP_VERSION/fpm/
COPY nginx/default /etc/nginx/sites-enabled/default

RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www
WORKDIR /app


ENTRYPOINT ["/docker-entrypoint.sh"]
