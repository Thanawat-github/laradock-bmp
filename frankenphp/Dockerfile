ARG FRANKENPHP_VERSION=1-php8
FROM dunglas/frankenphp:${FRANKENPHP_VERSION}

LABEL maintainer="Laradock"

# FrankenPHP ships mlocati's install-php-extensions helper. Install the
# extensions a typical Laravel app needs; add more here as required.
# pecl.php.net is flaky (504 / "No releases available"), so retry; already-installed extensions are skipped.
RUN for i in 1 2 3 4 5; do \
      install-php-extensions \
        pdo_mysql \
        pdo_pgsql \
        redis \
        intl \
        zip \
        gd \
        bcmath \
        pcntl \
        opcache \
        exif \
      && exit 0; \
      echo "install-php-extensions: attempt $i failed, retrying..." >&2; \
      sleep 10; \
    done; \
    exit 1

# OPcache/JIT tuning for the Octane worker, see php.ini.
COPY ./php.ini /usr/local/etc/php/conf.d/zz-app.ini

WORKDIR /app
