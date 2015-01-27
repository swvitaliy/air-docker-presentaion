FROM ubuntu:14.04
RUN apt-get update; apt-get -y dist-upgrade; \
 apt-get -y install php5-cli php5-common php5-mysql \
 php5-gd php5-fpm php5-cgi php5-fpm php-pear php5-mcrypt nginx \
 && echo "cgi.fix_pathinfo = 0;" >> /etc/php5/fpm/php.ini  \
 && rm /usr/share/nginx/html/index.html ; mkdir /etc/nginx/common
ADD upstream /etc/nginx/common/upstream
ADD default /etc/nginx/sites-available/default
ADD index.php /usr/share/nginx/html/index.php
ADD run.sh /run.sh
RUN chmod +x /run.sh
EXPOSE 80
CMD ["/run.sh"]
