FROM php:8.1.8-fpm

# Install necessary packages
RUN apt-get update && apt-get install -y git libzip-dev zip unzip vim

# Install PHP extensions
RUN docker-php-ext-install zip pdo_mysql bcmath
RUN apt-get update && apt-get install -y libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

ENV PATH="/usr/bin:/usr/local/bin:/usr/local/sbin:/usr/sbin:/sbin:${PATH}"

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_NO_INTERACTION 1

# Copy project files
COPY . /var/www/html/api/
WORKDIR /var/www/html/api

# Set up permissions and install dependencies
# RUN chmod -R a+w storage/ bootstrap/cache

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
# RUN composer install

CMD php artisan serve --host=0.0.0.0 --port=8080