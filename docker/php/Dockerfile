FROM php:7.1-apache

# Get updated packages list
RUN apt-get update

# Install packages
RUN  apt-get install -y git
RUN  apt-get install -y libfreetype6-dev
RUN  apt-get install -y libmcrypt-dev
RUN  apt-get install -y libicu-dev
RUN  apt-get install -y libjpeg-dev
RUN  apt-get install -y liblua5.2
RUN  apt-get install -y liblua5.2-dev
RUN  apt-get install -y libpng12-dev
RUN  apt-get install -y libzip-dev
RUN  apt-get install -y lua5.2
RUN  apt-get install -y vim
RUN  apt-get install -y wget
RUN  apt-get install -y zip

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
RUN pecl install lua
RUN pecl install xdebug
RUN pecl install zip

#Enable pecl extension
RUN docker-php-ext-enable redis
RUN docker-php-ext-enable lua
RUN docker-php-ext-enable xdebug
RUN docker-php-ext-enable zip
