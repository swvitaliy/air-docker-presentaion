title: Docker в веб-разработке
output: index.html

--

## Docker в веб-разработке


<p style="text-align: center">
	<img src="http://blog.docker.com/media/Screen-Shot-2014-09-16-at-6.26.20-AM.png" width="600px">
</p>

--

## Lamp. apt-get

    FROM ubuntu:14.04
    RUN apt-get update; \
    	apt-get -y dist-upgrade; \
      apt-get -y install apache2 php5 php5-mysql 
      		mysql-server libapache2-mod-php5; \
          locale-gen ru
          
--

## Переменные окружения
          
    ENV LANG ru_RU.UTF-8
    ENV LANGUAGE ru_RU.UTF-8
    ENV APACHE_RUN_USER www-data
    ENV APACHE_RUN_GROUP www-data
    ENV APACHE_LOG_DIR /var/log/apache2
    
   
--

## Порты и директории

    EXPOSE 80
    VOLUME /var/www/html

   
--

## Запуск
   
    CMD ["service apache2 start && 
    		/usr/sbin/mysqld --character-set-server=utf8 
        		--collation-server=utf8_general_ci"]


--

## В каких еще случаях нам поможет docker?


<ul>
	<li>php5-fpm вместо apache</li>
	<li>Меняем mysql на mariadb</li>
</ul>

--

## php5-fpm вместо apache

<ul>
	<li>создаем контейнер для fpm</li>
	<li>собираем связку контейнеров</li>
</ul>

* контейнер баз данных mysql мы оставляем неизменным

--

## Меняем mysql на mariadb

    FROM ubuntu:14.04
    RUN apt-key adv --keyserver ... && \
        echo "deb ...." > /etc/apt/sources... && \
        apt-get update && \
        DEBIAN_FRONTEND=noninteractive \
              apt-get install -y mariadb-server && \
    	...
    

--

## Легкость переноса образов и контейнеров

<ul>
	<li>собрали</li>
  <li class="next">протестировали</li>
  <li class="next">развернули</li>
</ul>

--

## That's all, folks!

<span>Виталий Сверчков & Юрий Сафаргалиев</span>

<span>Tech Meetup: технические встречи АИР</span>

<span>Саранск, 2014</span>