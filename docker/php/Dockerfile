FROM php:7.4-fpm-alpine

LABEL maintainer="Janosch Kocsis <jk@coloso.de>"

# install apcu
RUN apk add --update --no-cache --virtual .build-dependencies $PHPIZE_DEPS \
        && pecl install apcu \
        && docker-php-ext-enable apcu \
        && pecl clear-cache \
        && apk del .build-dependencies

RUN ( \
  echo "extension=apcu.so"; \
  echo "apc.enabled=1"; \
  ) > /usr/local/etc/php/conf.d/ext-apcu.ini

CMD docker pull node

RUN apk add --update \
    make \
    curl \
    nodejs \
    yarn \
    git \
    npm

RUN docker-php-ext-install \
    mysqli \
    pdo \
    pdo_mysql \
    opcache

RUN docker-php-ext-enable \
	opcache

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    ln -s $(composer config --global home) /root/composer
ENV PATH=$PATH:/root/composer/vendor/bin COMPOSER_ALLOW_SUPERUSER=1

ADD php.ini /etc/php/conf.d/
ADD php.ini /etc/php/cli/conf.d/
ADD php-fpm.conf /etc/php/php-fpm.d/
# ADD xdebug.ini  /etc/php/conf.d/

WORKDIR /var/www/symfony

EXPOSE 9000

CMD ["php-fpm", "-F"]


