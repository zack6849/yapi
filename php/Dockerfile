FROM php:7-fpm

# Setup NodeSource 13.x Repositories
RUN curl -sL https://deb.nodesource.com/setup_13.x | bash -
# Install yarn repo
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
# Install mysql-client, git, node golang (for mhsendmail), etc.
RUN apt-get update && apt-get install -y libldap2-dev libzip-dev libfreetype6-dev libpng-dev libjpeg62-turbo-dev zlib1g-dev locales libicu-dev mariadb-client git nodejs golang yarn vim nano unzip sudo iproute2 inetutils-ping redis
# Install mhsendmail (to send mail to mailhog)
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail

# Copy mailhog.ini to php conf directory
COPY mailhog.ini /usr/local/etc/php/conf.d/

# Set Locale, LANG and Timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/America/New_York /etc/localtime
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN pecl install xdebug-2.8.1 &&  yes '' | pecl install redis && docker-php-ext-enable xdebug && docker-php-ext-enable redis
# Install xdebug, not enabled yet.
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu
RUN docker-php-ext-install pdo_mysql bcmath zip gd intl pcntl ldap

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir /usr/bin --filename composer
RUN composer --version

COPY laravel_sudoers /etc/sudoers.d/
COPY docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN chmod 0440 /etc/sudoers.d/laravel_sudoers

# Create Laravel User
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}
RUN addgroup -gid ${PGID} laravel
RUN useradd -Nms /bin/bash laravel --uid ${PUID} -G laravel,sudo
USER laravel
WORKDIR /var/www/html

COPY docker-php-entrypoint.sh /usr/local/bin/
#ENTRYPOINT ["ocker-php-entrypoint.sh"]
