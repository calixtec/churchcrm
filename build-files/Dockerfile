FROM php:8-apache

EXPOSE 80

# Install necessary packages and extensions
RUN apt-get update && \
    apt-get install -y \
        libxml2-dev \
        gettext \
        libpng-dev \
        libzip-dev \
        libfreetype6-dev \
        libmcrypt-dev \
        libjpeg-dev \
        npm \
        git && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install -j$(nproc) xml exif pdo_mysql gettext iconv mysqli zip gd && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Copy configuration files
#COPY ./apache/default.conf /etc/apache2/apache2.conf
COPY httpd.conf /etc/apache2
COPY 60-churchcrm /etc/run_always

# Enable Apache modules
RUN a2enmod rewrite

# Configure PHP settings
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" && \
    sed -i 's/^upload_max_filesize.*$/upload_max_filesize = 2G/g' $PHP_INI_DIR/php.ini && \
    sed -i 's/^post_max_size.*$/post_max_size = 2G/g' $PHP_INI_DIR/php.ini && \
    sed -i 's/^memory_limit.*$/memory_limit = 2G/g' $PHP_INI_DIR/php.ini && \
    sed -i 's/^max_execution_time.*$/max_execution_time = 120/g' $PHP_INI_DIR/php.ini

# Create application directory
RUN mkdir -p /web/churchcrm

# Set file permissions
RUN chmod +x /etc/run_always/60-churchcrm

# Set environment variables
ENV TERM xterm
