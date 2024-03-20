FROM php:7.4-apache-buster

USER root

WORKDIR /var/www/html

# Get repository and install wget and vim
RUN apt-get update && apt-get install -y \
    wget \
    apt-utils \
    gnupg \
    software-properties-common \
    apt-transport-https \
    unixodbc-dev

# Install dependencies
RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    zlib1g-dev \
    libicu-dev \
    g++ \
    && ACCEPT_EULA=Y apt-get install --no-install-recommends -y msodbcsql17 mssql-tools \
    && apt-get install --no-install-recommends -y libxml2-dev \
    libaio-dev \
    libmemcached-dev \
    freetds-dev \
    libssl-dev \
    openssl \
    supervisor

RUN apt-get update && apt-get install -y \
    libzip-dev \
    libonig-dev \
    zip \
    curl \
    unzip \
    gnupg2 \
    && docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-source delete

COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf

RUN wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && wget -qO - https://packages.microsoft.com/config/debian/9/prod.list \
    > /etc/apt/sources.list.d/mssql-release.list

RUN pecl install sqlsrv pdo_sqlsrv && docker-php-ext-enable sqlsrv pdo_sqlsrv

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN chmod -R 777 /var/www/html \
    && a2enmod rewrite

# Cria link simbólico para o phpunit, tornando-o global
RUN cd /usr/bin \
    && ln -s /var/www/html/vendor/bin/phpunit phpunit