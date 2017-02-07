FROM php:apache

MAINTAINER DACRepair 

RUN apt-get update \
    && apt-get -y --no-install-recommends install \
        git g++ libicu52 libicu-dev zlib1g-dev \
        libxml2-dev libldap2-dev \
        imagemagick

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-configure intl
    && docker-php-ext-install mysqli opcache ldap xml mbstring intl

RUN apt-get purge -y --auto-remove g++ libicu-dev zlib1g-dev libxml2-dev libldap2-dev \
    && rm -rf /var/lib/apt/lists/*

RUN a2enmod rewrite

RUN git clone https://github.com/wikimedia/mediawiki.git /var/www/html

COPY apache/mediawiki.conf /etc/apache2/

RUN echo Include /etc/apache2/mediawiki.conf >> /etc/apache2/apache2.conf

COPY docker-entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["apache2-foreground"]
