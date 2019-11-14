FROM php:5.6-fpm
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
    sed -i \
        -e 's/deb.debian.org/mirrors.aliyun.com/g' \
        -e 's/security.debian.org/mirrors.aliyun.com/g' \
        /etc/apt/sources.list && \
    apt-get update && \
    apt-get -y install \
        gnupg2 && \
    apt-key update && \
    apt-get update && \
    apt-get -y install \
            g++ \
            git \
            curl \
            imagemagick \
            libyaml-dev \
            libcurl3-dev \
            libicu-dev \
            libfreetype6-dev \
            libjpeg-dev \
            libjpeg62-turbo-dev \
            libmagickwand-dev \
            libpq-dev \
            libpng-dev \
            libxml2-dev \
            libzip-dev \
            zlib1g-dev \
            default-mysql-client \
            openssh-client \
            nano \
            unzip \
            libcurl4-openssl-dev \
            libssl-dev \
        --no-install-recommends && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
        mkdir -p /var/www/html
        
RUN docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-configure sysvshm && \
    docker-php-ext-install \
        soap \
        zip \
        curl \
        bcmath \
        exif \
        gd \
        iconv \
        intl \
        mbstring \
        opcache \
        pdo_mysql \
        pdo_pgsql
        
        
# Install PECL extensions
# see http://stackoverflow.com/a/8154466/291573) for usage of `printf`
#RUN printf "\n" | pecl install \
#        imagick \
#        mongodb \
#        yaml && \
#    docker-php-ext-enable \
#        imagick \
#        mongodb \
#        yaml

# Check if Xdebug extension need to be compiled
#RUN cd /tmp && \
#    git clone git://github.com/xdebug/xdebug.git && \
#    cd xdebug && \
#    git checkout 2.7.2 && \
#    phpize && \
#    ./configure --enable-xdebug && \
#    make && \
#    make install && \
#    rm -rf /tmp/xdebug

# Environment settings
ENV PHP_USER_ID=33 \
#    PHP_ENABLE_XDEBUG=0 \
    PATH=/var/www/html/vendor/bin:/root/.composer/vendor/bin:$PATH \
    TERM=linux \
    GITHUB_API_TOKEN=9c21de6eac4c969cbb9970fb3e1565c6b23041ca \
    VERSION_PRESTISSIMO_PLUGIN=^0.3.9 \
    COMPOSER_ALLOW_SUPERUSER=1

# Add configuration files
COPY image-files/ /

# Add GITHUB_API_TOKEN support for composer
RUN chmod 700 \
        /usr/local/bin/docker-php-entrypoint \
        /usr/local/bin/composer

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer.phar \
        --install-dir=/usr/local/bin && \
    composer clear-cache && \
    composer config -g repo.packagist composer https://packagist.phpcomposer.com

# Install composer plugins
RUN composer global require --optimize-autoloader \
        "hirak/prestissimo:${VERSION_PRESTISSIMO_PLUGIN}" && \
    composer global dumpautoload --optimize && \
    composer clear-cache

# Enable mod_rewrite for images with apache
RUN if command -v a2enmod >/dev/null 2>&1; then \
        a2enmod rewrite headers \
    ;fi
    

