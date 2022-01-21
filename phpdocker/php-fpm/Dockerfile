FROM phpdockerio/php74-fpm:latest

EXPOSE 3306
EXPOSE 5432

WORKDIR "/application"



# Fix debconf warnings upon build

ARG DEBIAN_FRONTEND=noninteractive



# Install selected extensions and other stuff

RUN apt-get update \

#&& apt-get -y --no-install-recommends install php7.4-mysql php7.4-gd php7.4-intl php-yaml nginx vim nano supervisor
&& apt-get -y --no-install-recommends install php8.1-pgsql php7.4-gd php7.4-intl php-yaml nginx vim nano supervisor


# && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN rm /etc/nginx/sites-enabled/default

ADD . ./

ADD ./phpdocker/php-fpm/php-ini-overrides.ini /etc/php/7.4/fpm/conf.d/99-overrides.ini

ADD ./phpdocker/nginx/nginx.conf /etc/nginx/conf.d/default.conf

COPY ./phpdocker/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

#RUN composer config platform.php 7.2.5

#RUN composer update --ignore-platform-reqs
RUN composer install --ignore-platform-reqs
# --ignore-platform-req=ext-zip --ignore-platform-req=ext-xml --ignore-platform-req=ext-dom --ignore-platform-req=ext-simplexml
 

#RUN ./bin/console doctrine:migrations:migrate -n

#RUN ./bin/console app:create-user --admin azhar azhar@veersatech.com azhar123

#CMD ["nginx","-g", "daemon off;"]

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
