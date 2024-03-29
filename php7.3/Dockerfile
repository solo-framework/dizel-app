FROM php:7.3-alpine

ENV PHP_SYSCONF_PATH=/etc/php7

# https://github.com/codecasts/php-alpine
# https://github.com/docker-library/docs/tree/15828211b562d617ad16fdb866b6fdc800c95c34/php#how-to-install-more-php-extensions

RUN set -ex; \
		apk --update upgrade --no-cache; \
		apk add --no-cache autoconf g++ libtool make php7-dev git php7-pear tzdata zlib-dev openssl-dev ;\
		apk add --no-cache \
	        curl \
	        zlib \
	        ca-certificates \
	        openssl \
	        libmemcached \
	        rabbitmq-c \
	        php7 \
	        php7-dom \
	        php7-fpm \
	        php7-cgi \
	        php7-mbstring \
	        php7-mcrypt \
	        php7-opcache \
	        php7-pdo \
	        php7-pdo_pgsql \
	        php7-pdo_sqlite \
	        php7-fileinfo \
	        php7-xml \
	        php7-simplexml \
	        php7-xmlreader \
	        php7-xmlwriter \
	        php7-phar \
	        php7-openssl \
	        php7-json \
	        php7-curl \
	        php7-ctype \
	        php7-session \
	        php7-zlib \
	        php7-tokenizer \
			php7-xdebug; \


		pecl install mongodb redis xdebug; \
#		echo "extension=mongodb.so" > /etc/php7/conf.d/mongodb.ini; \
		docker-php-ext-enable redis mongodb xdebug; \
		pecl clear-cache; \
		docker-php-source delete;

# RUN cp /usr/share/zoneinfo/Europe/Moscow /etc/localtime; \
#     echo "Europe/Moscow" >  /etc/timezone; \
#     apk del tzdata;



ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp
ENV COMPOSER_VERSION 1.8.4

RUN curl --silent --fail --location --retry 3 --output /tmp/installer.php --url https://raw.githubusercontent.com/composer/getcomposer.org/cb19f2aa3aeaa2006c0cd69a7ef011eb31463067/web/installer \
 && php -r " \
    \$signature = '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5'; \
    \$hash = hash('sha384', file_get_contents('/tmp/installer.php')); \
    if (!hash_equals(\$signature, \$hash)) { \
        unlink('/tmp/installer.php'); \
        echo 'Integrity check failed, installer is either corrupt or worse.' . PHP_EOL; \
        exit(1); \
    }" \
 && php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} \
 && composer --ansi --version --no-interaction \
 && rm -f /tmp/installer.php \
 && composer self-update;


RUN cp /usr/share/zoneinfo/Etc/UTC /etc/localtime; \
    echo "UTC" >  /etc/timezone;\
    apk del tzdata;

# RUN echo "zend_extension=xdebug.so" > /etc/php7/conf.d/xdebug.ini;