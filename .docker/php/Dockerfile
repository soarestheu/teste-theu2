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

COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf

RUN wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && wget -qO - https://packages.microsoft.com/config/debian/9/prod.list \
    > /etc/apt/sources.list.d/mssql-release.list

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN chmod -R 777 /var/www/html \
    && a2enmod rewrite

# Cria link simbólico para o phpunit, tornando-o global
RUN cd /usr/bin \
    && ln -s /var/www/html/vendor/bin/phpunit phpunit

CMD [ "composer", "install" ]