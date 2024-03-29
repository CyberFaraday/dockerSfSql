FROM php:8.3-fpm

ARG GIT_USEREMAIL
ARG GIT_USERNAME
ENV GIT_USEREMAIL $GIT_USEREMAIL
ENV GIT_USERNAME $GIT_USERNAME

ARG CACHEBUST=1
ENV NODE_VERSION=--lts

RUN apt update \
    && apt install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip nano vim \
    && docker-php-ext-install intl opcache pdo pdo_mysql exif \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip


RUN apt-get update && \
    apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev --no-install-recommends --fix-missing\
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/\
    && docker-php-ext-install gd



RUN pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.mode=debug,coverage" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


RUN curl -sS 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash
RUN apt install symfony-cli


RUN git config --global user.email "${GIT_USEREMAIL}" \
    && git config --global user.name "${GIT_USERNAME}"


RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -qq -y build-essential yarn wget

RUN apt-get install -y postgresql postgresql-contrib postgresql-server-dev-15 

#for postgresql php drivers
RUN  wget http://php.net/distributions/php-8.3.1.tar.gz \
    && PHP_EXT_DIR=$( php-config --extension-dir ) \
    && tar -xvzf php-8.3.1.tar.gz -C /root \
    && cd /root/php-8.3.1/ext/pgsql && phpize \ 
    && ./configure -with-pgsql=/usr/lib/postgresql/15/bin \
    && make && mv modules/pgsql.so $PHP_EXT_DIR \
    && cd /root/php-8.3.1/ext/pdo_pgsql \ 
    && phpize \
    && ./configure -with-pgsql=/usr/lib/postgresql/15/bin \
    && make && mv modules/pdo_pgsql.so $PHP_EXT_DIR \
    && rm -r /root/php-8.3.1 \
    && apt-get remove -y --purge postgresql postgresql-contrib postgresql-server-dev-15 g++ wget build-essential

#COPY docker-php-ext-pdo_pgsql.ini /usr/local/etc/php/conf.d/docker-php-ext-pdo_pgsql.ini
RUN touch /usr/local/etc/php/conf.d/docker-php-ext-pdo_pgsql.ini
RUN echo "extension=pdo_pgsql.so" > /usr/local/etc/php/conf.d/docker-php-ext-pdo_pgsql.ini
COPY php.ini /usr/local/etc/php/php.ini
COPY .bashrc /root/.bashrc


RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash \
    && echo "source /root/.bashrc && nvm install $NODE_VERSION"




WORKDIR /var/www/app
#/usr/local/lib/php/extensions/no-debug-non-zts-20230831 
# in the variable  $PHP_EXT_DIR at this day (PHP_EXT_DIR=$( php-config --extension-dir ))
# for dynamics updates whith this Dockerfile
