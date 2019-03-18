FROM php:7.2-fpm
LABEL maintainer="gzp@goozp.com"


# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install package and PHP Core extensions
RUN apt-get update && apt-get install -y \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        vim \
        openssl \
        libssl-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && rm -r /var/lib/apt/lists/*

# Copy extensions had downloaded
COPY ./pkg/redis.tgz /home/redis.tgz
#COPY ./pkg/cphalcon.tar.gz /home/cphalcon.tar.gz
COPY ./pkg/xdebug-2.7.0.tgz /home/xdebug-2.7.0.tgz
COPY ./pkg/swoole-4.2.9.tgz /home/swoole-4.2.9.tgz

# Install PECL extensions (Redis)
RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# Install Phalcon extensions
#RUN cd /home \
#    && tar -zxvf cphalcon.tar.gz \
#    && mv cphalcon-* phalcon \
#   && cd phalcon/build \
#    && ./install \
#    && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini

#安装xdebug扩展
RUN cd /home \
        && tar -zxvf xdebug-2.7.0.tgz \
        && cd xdebug-2.7.0 \
        && phpize \
        && ./configure \
        && make \
        && cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20170718 \
        && echo "zend_extension = /usr/local/lib/php/extensions/no-debug-non-zts-20170718/xdebug.so" > /usr/local/etc/php/php.ini

RUN touch /usr/local/etc/php/conf.d/xdebug.ini && \
    echo 'zend_extension = /usr/local/lib/php/extensions/no-debug-non-zts-20170718/xdebug.so \n xdebug.remote_enable = on \n xdebug.remote_connect_back = on \n xdebug.remote_handler = dbgp \n xdebug.remote_port = 9001 \n xdebug.remote_host = 192.168.33.16 \n xdebug.idekey = PHPSTORM' > /usr/local/etc/php/conf.d/xdebug.ini

#安装swoole扩展
RUN cd /home &&\
    tar zxvf swoole-4.2.9.tgz && \
        cd swoole-4.2.9  && \
        phpize  && \
        ./configure  --enable-openssl && \
        make && make install

RUN touch /usr/local/etc/php/conf.d/swoole.ini && \
    echo 'extension=swoole.so' > /usr/local/etc/php/conf.d/swoole.ini
# Install Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

RUN rm -f /home/redis.tgz \
#        rm -f /home/cphalcon.tar.gz
        rm -f /home/xdebug-2.7.0.tgz \
        rm -f /home/swoole-4.2.9.tgz
WORKDIR /data

# Write Permission
RUN usermod -u 1000 www-data
