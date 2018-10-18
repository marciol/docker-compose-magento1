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

RUN printf '* *\t* * *\tapp\t%s/usr/local/bin/php /var/www/html/cron.php\n#\n' >> /etc/crontab

COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/php-fpm.conf /usr/local/etc/
COPY bin/cronstart /usr/local/bin/

RUN mkdir /sock

VOLUME /var/www

WORKDIR /var/www/html

EXPOSE 9001

# Install Java.
RUN mkdir /usr/share/man/man1/
RUN mkdir /var/www/.cache

RUN \
  apt-get update && \
  apt-get install -y software-properties-common xvfb wget sudo gnupg && \
  add-apt-repository ppa:openjdk-r/ppa && \
  apt-get install -y openjdk-8-jdk && \
rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# Chrome and chromedriver
RUN apt-get update && \
    apt-get install -yq wget --no-install-recommends && \
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' && \
    apt-get update && \
    apt-get install -y chromedriver google-chrome-unstable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst ttf-freefont libgconf-2-4 --no-install-recommends

RUN apt-get install ifconfig

ENTRYPOINT ["docker-php-entrypoint", "-R"]