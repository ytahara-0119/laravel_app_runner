FROM centos:centos7.5.1804

RUN yum update -y \
    && yum install -y mariadb zip unzip xml-common \
    && yum install -y epel-release yum-utils \
    && yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm \
    && yum-config-manager --enable remi-php74 \
    && yum install -y gd gd-devel php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysqld php-mysqli \
    && yum install -y php-zip php74-php-imap php74-php-xml php-xml php74-php-zip php74-php-mbstring php-mbstring php-pecl-redis
    
RUN yum install -y git
RUN yum install -y httpd httpd-devel
RUN yum install -y mod_ssl
RUN yum install -y php-pear php-devel
RUN yum install -y php74-php-pecl-xdebug
RUN curl -fsSL https://rpm.nodesource.com/setup_14.x | bash -
RUN yum install -y nodejs
RUN npm update -g


# install composer
RUN cd '/' \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer


RUN chown -R apache:apache /var/www

WORKDIR /var/www/
#COPY composer.json .
#COPY composer.lock .
#COPY package.json .
#COPY package-lock.json .

RUN chown -R apache:apache /var/www
#RUN chmod 777 -R /var/www/html
RUN composer create-project --prefer-dist laravel/laravel sample "8.*"

RUN mv /var/www/sample/* /var/www/html/
RUN mv /var/www/sample/.editorconfig /var/www/html/
RUN mv /var/www/sample/.env /var/www/html/
RUN mv /var/www/sample/.env.example /var/www/html/
RUN mv /var/www/sample/.gitattributes /var/www/html/
RUN mv /var/www/sample/.gitignore /var/www/html/
RUN mv /var/www/sample/.styleci.yml /var/www/html/

RUN rm -rf /var/www/sample/

WORKDIR /var/www/html
RUN chown -R apache:apache /var/www/html
RUN chmod -R 777 storage
RUN chmod -R 777 bootstrap/cache
COPY env-example .env
RUN touch database/database.sqlite
RUN php artisan migrate

#RUN composer install
#RUN npm install && npm run dev

CMD ["/usr/sbin/init"]
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
