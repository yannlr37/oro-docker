FROM php:8-fpm

WORKDIR /var/www/html/

RUN rm /etc/apt/preferences.d/no-debian-php \
    && apt-get --allow-releaseinfo-change update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libmcrypt-dev \
        libmagickwand-dev \
        libxml2-dev \
        zlib1g-dev \
        imagemagick \
        libzip-dev \
        libpq-dev \
        libldap2-dev \
        libtidy-dev \
        jpegoptim \
        unzip \
        git \
        msmtp \
        supervisor \
        cron \
        vim \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
	&& docker-php-ext-install -j$(nproc) soap \
	&& docker-php-ext-install -j$(nproc) zip \
	&& docker-php-ext-install -j$(nproc) intl \
	&& docker-php-ext-install -j$(nproc) pcntl \
	&& docker-php-ext-install -j$(nproc) ldap \
	&& CFLAGS="$CFLAGS -D_GNU_SOURCE" docker-php-ext-install -j$(nproc) sockets \
	&& docker-php-ext-install -j$(nproc) tidy \
    && pecl install xdebug && docker-php-ext-enable xdebug \
    && apt-get -y autoremove && apt-get clean

## mongoDb needed from ORO
RUN pecl install mongodb && docker-php-ext-enable mongodb

## COMPOSER INSTALL
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --2 --install-dir=/usr/bin/ --filename=composer \
    && php -r "unlink('composer-setup.php');"

## CRON
COPY ./oro_cron /etc/cron.d/oro_cron
RUN chmod 0644 /etc/cron.d/oro_cron

RUN mkdir /var/log/cron
RUN touch /var/log/cron/oro_cron.log
RUN chown -R www-data:www-data /var/log

## NODE
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash -  && apt-get install -y nodejs;

## MISC
COPY ./php.ini /usr/local/etc/php/
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN usermod -u 1000 www-data

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
