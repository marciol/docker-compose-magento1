FROM nginx:1.13
MAINTAINER Mark Shust <mark@shust.com>

RUN touch /var/run/nginx.pid
RUN mkdir /sock

COPY ./conf/nginx.conf /etc/nginx/
COPY ./conf/default.conf /etc/nginx/conf.d/

VOLUME /var/www

WORKDIR /var/www/html

COPY ./conf/default.conf /etc/nginx/conf.d/