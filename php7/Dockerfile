FROM php:7.0.5-fpm

MAINTAINER thomas <long_kissfu@163.com>
RUN apt-get update && apt-get install -y \ 
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt pdo_mysql mysqli\
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd


RUN apt-get -y install libmemcached-dev libmemcached11 zlib1g-dev
RUN apt-get -y install git gcc wget

#INSTALL php-memcached
WORKDIR /tmp
RUN git clone https://github.com/php-memcached-dev/php-memcached && cd php-memcached && git checkout -b php7 origin/php7
WORKDIR /tmp/php-memcached
RUN /usr/local/bin/phpize 
RUN ./configure && make && make install 
RUN docker-php-ext-enable memcached

#INSTALL SOAP
RUN apt-get update && apt-get install -y libxml2-dev --no-install-recommends \
				   && docker-php-ext-install soap \ 
				   && rm -rf /var/lib/apt/lists/*


# Install redis
RUN wget -O /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/php7.tar.gz \
	&& mkdir -p /tmp/redis \
	&& tar xzf /tmp/redis.tar.gz -C /tmp/redis --strip-components=1 \
	&& cd /tmp/redis \
	&& /usr/local/bin/phpize \
	&& ./configure \
	&& make \
	&& make install \
	&& docker-php-ext-enable redis \
	&& rm -rf /tmp/redis.tar.gz /tmp/redis \
	&& rm -rf /var/lib/apt/lists/*

# Install imap
RUN apt-get update && apt-get -y install libssl-dev libc-client2007e-dev libkrb5-dev \
	--no-install-recommends \
	&& docker-php-ext-configure imap --with-imap-ssl --with-kerberos \
	&& docker-php-ext-install imap \
	&& rm -rf /var/lib/apt/lists/*

#Install ssh2
RUN apt-get update && apt-get install unzip libssh2-1-dev libssh2-1 -y --no-install-recommends \
    && wget -O /tmp/ssh2-php7.zip https://github.com/Sean-Der/pecl-networking-ssh2/archive/php7.zip \
    && unzip /tmp/ssh2-php7.zip -d /tmp/ssh2-php7 \
    && cd /tmp/ssh2-php7/pecl-networking-ssh2-php7 \
	&& /usr/local/bin/phpize \
	&& ./configure && make && make install \
	&& docker-php-ext-enable ssh2 \
	&& rm -rf /var/lib/apt/lists/*

#INSTALL gmp
RUN apt-get update && apt-get install libgmp-dev -y --no-install-recommends \
	&& ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
	&& docker-php-ext-install gmp \
	&& rm -rf /var/lib/apt/lists/*

#INSTALL intl
RUN apt-get update && apt-get install libicu-dev -y --no-install-recommends \
	&& docker-php-ext-install intl \
	&& rm -rf /var/lib/apt/lists/*
