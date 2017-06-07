FROM php:7.1.3-fpm

# Get updated packages list
RUN apt-get update

# Install packages
RUN  apt-get install -y \
        libmcrypt-dev \
        libicu-dev \
        lua5.2 \
        liblua5.2 \
        liblua5.2-dev \
        libjpeg-dev \
        libpng12-dev \
        libfreetype6-dev \
        vim \
        git \
        zip \
        wget

# Configure gd extension
RUN docker-php-ext-configure \
        gd \
            --enable-gd-native-ttf \
            --with-freetype-dir=/usr/include/freetype2 \
            --with-png-dir=/usr/include \
            --with-jpeg-dir=/usr/include

# Install php extensions
RUN docker-php-ext-install -j$(nproc) \
        mcrypt \
        mysqli \
        pdo_mysql \
        opcache \
        intl \
        gd \
        gettext \
        exif

# Symlink and copy lua files in the asumed locations that pecl install looks in.
RUN ln -s /usr/include/lua5.2/* /usr/include
RUN ln -s /usr/include/lua5.2 /usr/include/lua
RUN cp /usr/lib/x86_64-linux-gnu/liblua5.2.a /usr/lib/liblua.a
RUN cp /usr/lib/x86_64-linux-gnu/liblua5.2.so /usr/lib/liblua.so

# Install pecl extensions
RUN pecl install redis
RUN pecl install apcu
RUN pecl install apcu_bc-1.0.3
RUN pecl install lua
RUN pecl install xdebug
RUN pecl install zip

#Enable pecl extension
RUN docker-php-ext-enable redis
RUN docker-php-ext-enable lua
RUN docker-php-ext-enable apcu
RUN docker-php-ext-enable apc
RUN docker-php-ext-enable xdebug
RUN docker-php-ext-enable zip

# Rename ini files so the load in different order.
RUN mv /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini /usr/local/etc/php/conf.d/00-docker-php-ext-apcu.ini
RUN mv /usr/local/etc/php/conf.d/docker-php-ext-apc.ini /usr/local/etc/php/conf.d/10-docker-php-ext-apc.ini