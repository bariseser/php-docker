ARG PHP_VERSION

#base ubuntu
FROM bariseser/php-fpm:${PHP_VERSION}

LABEL maintainer="Bariseser <bariseser@gmail.com>"

ARG PHP_VERSION

RUN set -xe; \
    apt-get update -yqq \
    && pecl channel-update pecl.php.net \
    && apt-get install -yqq apt-utils libzip-dev zip unzip

#REDIS
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

#AMQP
RUN apt-get update && apt-get -y install cmake && \
    curl -L -o /tmp/rabbitmq-c.tar.gz https://github.com/alanxz/rabbitmq-c/archive/master.tar.gz && \
    mkdir -p rabbitmq-c && \
    tar -C rabbitmq-c -zxvf /tmp/rabbitmq-c.tar.gz --strip 1 && \
    cd rabbitmq-c/ && \
    mkdir _build && cd _build/ && \
    cmake .. && \
    cmake --build . --target install && \
    pecl install amqp && \
    docker-php-ext-enable amqp && \
    docker-php-ext-install sockets

#MySQL
USER root
RUN apt-get update -yqq && \
    apt-get -y install default-mysql-client

RUN set -xe; php -v | head -n 1 | grep -q "PHP ${PHP_VERSION}."

USER root
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
