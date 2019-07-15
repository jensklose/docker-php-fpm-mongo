FROM php:7.3-fpm

WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends --allow-unauthenticated \
  autoconf \
  pkg-config \
  libssl-dev \
  build-essential \
  libmcrypt-dev \
  libpng-dev \
  libjpeg62-turbo-dev \
  libfreetype6-dev \
  locales \
  zip \
  jpegoptim optipng pngquant gifsicle \
  vim \
  unzip \
  git \
  ssmtp/unstable \
  curl

# Install extensions
RUN docker-php-ext-install bcmath mbstring zip mcrypt
RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-install gd
RUN pecl install mongodb
RUN echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
