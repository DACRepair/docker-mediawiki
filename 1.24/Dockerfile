FROM php:5.6-apache
MAINTAINER DACRepair 

RUN apt-get update

RUN apt-get install -y git

RUN apt-get install -y --no-install-recommends \
        g++ \
        libicu52 \
        libicu-dev \
    && pecl install intl \
    && echo extension=intl.so >> /usr/local/etc/php/conf.d/ext-intl.ini \
    && apt-get purge -y --auto-remove g++ libicu-dev

RUN apt-get install libldap2-dev -y \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/
    
RUN docker-php-ext-install mysqli opcache ldap xml mbstring
RUN apt-get install -y --no-install-recommends imagemagick
    
# Clear the apt cache
RUN rm -rf /var/lib/apt/lists/*

RUN a2enmod rewrite

RUN git clone https://github.com/wikimedia/mediawiki.git /var/www/html

COPY apache/mediawiki.conf /etc/apache2/
RUN echo Include /etc/apache2/mediawiki.conf >> /etc/apache2/apache2.conf

COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]
