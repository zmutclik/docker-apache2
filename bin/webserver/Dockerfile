FROM php:7.4.1-apache

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y

RUN apt-get install -y \
        default-libmysqlclient-dev \
        libbz2-dev \
        libmemcached-dev \
        libsasl2-dev 

RUN apt-get install -y \
        apt-utils \
        nano \
        curl \
        dialog \
        git \
        wget \
        libfreetype6-dev \
        libcurl4 \
        libcurl4-openssl-dev \
        libicu-dev \
        libjpeg-dev \
        libjpeg62-turbo-dev \
        libldap2-dev \
        libmemcachedutil2 \
        libpng-dev \
        libpq-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        zip

# Install xdebug
RUN pecl install xdebug-2.9.0
RUN docker-php-ext-enable xdebug

# Other PHP7 Extensions
RUN apt-get -y install gcc make autoconf libc-dev pkg-config
RUN apt-get -y install libmcrypt-dev
RUN pecl install mcrypt-1.0.3

RUN docker-php-ext-install bcmath calendar iconv intl mbstring soap zip

RUN apt-get -y install mariadb-client
RUN docker-php-ext-install  mysqli opcache pdo_mysql pdo_pgsql pgsql

RUN docker-php-ext-configure gd --with-freetype --with-jpeg 
RUN docker-php-ext-install gd 

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu
RUN docker-php-ext-install ldap

RUN docker-php-ext-install curl
RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install json

RUN docker-php-ext-install exif
RUN pecl install memcached redis
RUN docker-php-ext-enable memcached.so redis.so

RUN apt-get -y install zlib1g-dev
RUN apt-get -y install libzip-dev
RUN docker-php-ext-install zip

RUN apt-get -y install libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

# Enable apache modules
RUN a2enmod rewrite headers
RUN a2enmod proxy proxy_http proxy_ajp deflate proxy_balancer proxy_connect proxy_html

# CUPS
RUN apt -y install cups
RUN /etc/init.d/cups start && cupsctl --remote-admin --remote-any --share-printers

RUN apt-get install -y imagemagick
RUN rm /etc/ImageMagick-6/policy.xml

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && ln -s $(composer config --global home) /root/composer
ENV PATH=$PATH:/root/composer/vendor/bin COMPOSER_ALLOW_SUPERUSER=1

COPY boot.sh /boot/boot.sh
RUN chmod +x /boot/boot.sh
CMD ["/boot/boot.sh"]