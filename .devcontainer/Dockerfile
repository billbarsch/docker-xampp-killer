FROM php:7.4-apache AS xamppdev

RUN apt-get update

# 1. development packages
RUN apt-get install -y \
    mariadb-client \
    git \
    zip \
    curl \
    sudo \
    unzip \
    zlib1g-dev \
    libxpm-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    libbz2-dev \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    libonig-dev \
    libmcrypt-dev \
    libreadline-dev \
    libfreetype6-dev \
    libcurl4-gnutls-dev \
    g++ \
    nano

# 2. install curl and node 14
RUN apt-get install curl -y \
    && curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y \
    nodejs

# 3. apache configs + document root
#ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
#RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
#RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# 4. mod_rewrite for URL rewrite and mod_headers for .htaccess extra headers like Access-Control-Allow-Origin-
RUN a2enmod rewrite headers

# 5. start with base php config, then add extensions
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp
RUN docker-php-ext-install \
    gd \
    bz2 \
    intl \
    iconv \
    bcmath \
    opcache \
    calendar \
    mbstring \
    curl \
    soap \
    xml \
    mysqli \
    pdo \
    pdo_mysql \
    zip

# 6. install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer