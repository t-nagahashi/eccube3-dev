FROM php:5.6.25-apache

# apt-get
RUN apt-get update && apt-get install -y \
    apache2-dev \
    libpq-dev \
    libmysqlclient-dev \
    postgresql-9.4 \
    git \
    zip \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# UTF-8
RUN localedef -i ja_JP -c -f UTF-8 -A /usr/share/locale/locale.alias ja_JP.UTF-8

# Install php extensions
RUN docker-php-ext-install pdo pdo_pgsql pdo_mysql zip

# Enabled mod_rewrite
RUN a2enmod rewrite

RUN { \
        echo 'date.timezone = "Asia/Tokyo"'; \
        echo 'post_max_size = "30M"'; \
        echo 'upload_max_filesize = "30M"'; \
    } > /usr/local/etc/php/conf.d/eccube.ini

# Xdebug
RUN pecl install xdebug && { \
        echo 'xdebug.remote_enable=1'; \
        echo 'xdebug.remote_host=10.254.254.254'; \
        echo 'xdebug.remote_port="9000"'; \
        echo 'xdebug.profiler_enable=0'; \
        echo 'xdebug.max_nesting_level=1000'; \
        echo 'xdebug.idekey = "IntelliJ"'; \
        echo 'xdebug.remote_connect_back=0'; \
        echo 'xdebug.remote_autostart=0'; \
        echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)"; \
    } > /usr/local/etc/php/conf.d/xdebug.ini

# Enable SSL
RUN a2enmod ssl
RUN ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/default-ssl.conf
EXPOSE 443

WORKDIR /var/www/html/ec-cube
RUN chown -R www-data: /var/www/html/ec-cube
