# Using the php 7.3 fpm base image
FROM php:7.4-fpm

# Update the apt-get
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    # Node and NPM
    nodejs \
    npm \
    # Needed for php intl extension
    zlib1g-dev \
    libicu-dev \
    g++ \
    lsb-release

# Install the mysql package repository
RUN wget https://dev.mysql.com/get/mysql-apt-config_0.8.16-1_all.deb
RUN DEBIAN_FRONTEND=noninteractive dpkg -i mysql-apt-config_0.8.16-1_all.deb
# MySQL (To use the command line client)
RUN apt update && apt install -y mysql-client
RUN rm mysql-apt-config_0.8.16-1_all.deb

# install php extensions
RUN docker-php-ext-install pdo pdo_mysql sockets pcntl intl

# Install composer
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Default working directory
WORKDIR /var/www

# Copy the source code
COPY . /var/www

# Copy dependencies for npm and install
COPY package.json /var/www
RUN npm install --production

# Copy the dependancies for composer and install
COPY composer.json /var/www
RUN composer install

# Default command when server starts
CMD ["php-fpm"]