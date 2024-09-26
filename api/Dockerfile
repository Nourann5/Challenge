FROM php:8.2-cli-buster

# Install system dependencies for PHP extensions and Composer
RUN  apt-get update && apt-get install -y \
        git \
        unzip \
        curl \
        libpng-dev \
        libonig-dev \
        libxml2-dev \
        zip \
        libzip-dev \
        default-mysql-client \
        default-libmysqlclient-dev

        # Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip
RUN rm -f libphp.la      modules/* libs/*
RUN rm -f ext/opcache/jit/zend_jit_x86.c
RUN rm -f ext/opcache/jit/zend_jit_arm64.c
RUN rm -f ext/opcache/minilua


# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set the working directory inside the container
WORKDIR /var/www

# Copy the application code to the container
COPY . /var/www

# Install PHP dependencies
RUN composer install --ignore-platform-reqs

# Expose port 8000 for the application
EXPOSE 8000

# Command to start the PHP server
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]