FROM php:8.0-apache
RUN a2enmod rewrite
RUN apt-get update
RUN apt-get install -y autoconf pkg-config libssl-dev
# add certificate and enable SSL
ADD mycert.key /etc/ssl/private/
ADD mycert.crt /etc/ssl/certs/
RUN a2enmod ssl && a2enmod socache_shmcb
RUN sed -i '/SSLCertificateFile.*snakeoil\.pem/c\SSLCertificateFile \/etc\/ssl\/certs\/mycert.crt' /etc/apache2/sites-available/default-ssl.conf
RUN sed -i '/SSLCertificateKeyFile.*snakeoil\.key/cSSLCertificateKeyFile /etc/ssl/private/mycert.key\' /etc/apache2/sites-available/default-ssl.conf
RUN a2ensite default-ssl
COPY "./php.ini" "$PHP_INI_DIR/php.ini"

RUN service apache2 restart

# Install the PHP extensions:xml y xdebug y mysqli
RUN  apt-get install -y wget
RUN wget https://github.com/FriendsOfPHP/pickle/releases/download/v0.7.9/pickle.phar \
    && mv pickle.phar /usr/local/bin/pickle \
    && chmod +x /usr/local/bin/pickle
    
RUN pickle install xdebug
RUN pickle install mysqli

RUN apt-get install -y libxml2-dev

ADD xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

CMD ["apache2-foreground"]




