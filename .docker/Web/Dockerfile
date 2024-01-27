# Use the official PHP 8.1 FPM image as the base image
FROM php:8.1-fpm

# Set the working directory to /var/www
WORKDIR /var/www

# Update package lists and install required packages
RUN apt-get update && \
    apt-get install -y \
    bash \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    curl

# Configure and install PHP extensions (gd, zip, pdo, pdo_mysql)
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd zip pdo pdo_mysql

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy the Laravel application files from the parent directory into the container
COPY /Web/. /var/www/

# Expose ports 8000 and 9000 (you may need to map them when running the container)
EXPOSE 8000 9000

# Copy the entrypoint script into the container and make it executable
COPY /.docker/Web/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

# Define the entry point script to run when the container starts
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]