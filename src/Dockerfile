FROM php:alpine

ARG INSTALLER_BASE_URL="https://github.com/mlocati/docker-php-extension-installer/releases"
ARG SCHEDULE_CRONTAB="* * * * * /usr/local/bin/php /var/www/artisan schedule:run"

ADD $INSTALLER_BASE_URL/latest/download/install-php-extensions /usr/local/bin/

ENV TZ=Europe/Berlin

### default laravel octane dependencies
RUN crontab -l | { cat; echo "$SCHEDULE_CRONTAB"; } | crontab - \
    && chmod +x /usr/local/bin/install-php-extensions \
    && apk update && apk upgrade && apk add --update \
    mariadb-client \
    supervisor \
    xdg-utils \
    pngcrush \
    tzdata \
    nodejs \
    npm

### build essentials for node canvas
RUN apk add --update --no-cache \
    make \
    g++ \
    jpeg-dev \
    cairo-dev \
    giflib-dev \
    pango-dev \
    libtool \
    autoconf \
    automake

### required php dependencies
RUN install-php-extensions \
    @composer \
    apcu \
    bcmath \
    gd \
    imagick/imagick@master \
    intl \
    memcached \
    opcache \
    pcntl \
    pdo_mysql \
    redis \
    swoole \
    zip

### set timezone and clean-up
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone \
    && apk del tzdata \
    && rm -rf /usr/local/bin/install-php-extensions \
    && rm -rf /tmp/* /var/tmp/* \
    && rm -rf /var/cache/apk/*

### default supervisor and php ini files
COPY super.conf /etc/supervisor/conf.d/supervisord.conf
COPY octane.ini /usr/local/etc/php/conf.d/octane.ini

WORKDIR /var/www

EXPOSE 9000 5173

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

### a Laravel installation with Octane is mandatory
HEALTHCHECK --start-period=5s --interval=2s --timeout=5s --retries=8 \
    CMD php artisan octane:status || exit 1
