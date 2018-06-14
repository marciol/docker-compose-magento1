FROM php:5.6-fpm

RUN apt-get update && apt-get install -y \
  cron \
  git \
  libfreetype6-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libpng-dev \
  libxslt1-dev \
  mysql-client \
  vim \
  zip

RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN docker-php-ext-install \
  bcmath \
  gd \
  intl \
  mbstring \
  mcrypt \
  opcache \
  pdo_mysql \
  soap \
  xsl \
  zip

RUN pecl channel-update pecl.php.net \
  && pecl install xdebug-2.5.5 \
  && docker-php-ext-enable xdebug \
  && sed -i -e 's/^zend_extension/\;zend_extension/g' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

RUN groupadd -g 1000 app \
 && useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app

RUN printf '* *\t* * *\tapp\t%s/usr/local/bin/php /var/www/html/cron.php\n#\n' >> /etc/crontab

COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/php-fpm.conf /usr/local/etc/
COPY bin/cronstart /usr/local/bin/

RUN mkdir /sock

VOLUME /var/www

WORKDIR /var/www/html

EXPOSE 9001

USER root

# Install Java.
RUN mkdir /usr/share/man/man1/
RUN mkdir /var/www/.cache

RUN \
  apt-get update && \
  apt-get install -y software-properties-common iceweasel xvfb wget && \
  add-apt-repository ppa:openjdk-r/ppa && \
  apt-get install -y openjdk-8-jdk && \
rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# Geckodriver
RUN wget https://github.com/mozilla/geckodriver/releases/download/v0.16.1/geckodriver-v0.16.1-linux64.tar.gz && \
    sh -c 'tar -x geckodriver -zf geckodriver-v0.16.1-linux64.tar.gz -O > /usr/bin/geckodriver' && \
    chmod +x /usr/bin/geckodriver && \
    rm geckodriver-v0.16.1-linux64.tar.gz

RUN groupadd docker && usermod -aG docker app

ENTRYPOINT ["docker-php-entrypoint", "-R"]