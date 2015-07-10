title: Docker в веб-разработке
output: index.html

--

## Docker в веб-разработке


<p style="text-align: center">
	<img src="/images/logo.png" style="height: 400px">
</p>

--

## Docker - это ...

* движок Docker Engine
* runtime для контейнеров
* инструментарий для создания пакетов
* API
* облачный сервис Docker Hub

--

## Docker - это ...

Высокопроизводительный движок, который автоматизирует развертывание любого приложения в **легкий**, **переносимый**, **самодостаточный** контейнер, который можно запустить где угодно.

--

## Общие случаи использования Docker

* Автоматический перенос и развертывание приложения.
* Создание легковесного, защищенного PAAS окружения.
* Автоматизация тестирования и CI/развертывания
* Развертывание и масштабирование веб приложений, баз данных и бэкенд серверов.

--

## Docker для разработчиков

**Build once...run anywhere**

Он дает простое изолированное окружение и повторное использование.

--

## Docker для DevOp

**Configure once...run anything**

* более эффективный цикл разработки и развертывания
* Убирает несоответствия между разными средами 
* увеличивает скорость и надежность развертывания
* возможно развертывание контейнеров внутри виртуальных машин.

--

## Некоторые крутые вещи

* Сделать свою собственную PaaS
* Окружение для веб приложения
* Упростить развертывание приложения
* Создайте безопасную песочницу для своих экспериментов
* Создайте свой SaaS
* Автоматический деплой
* Использование в CI 

--

## Примеры PaaS на docker

* octohost.io - `git push` деплой
* tutum.co - GUI
* deis.io - из командной строки
* dokku - `git push` деплой. self-hosted.
* drone.io - CI, реализованная с помощью docker
* flocker - система управления кластером

--

## SOA из вики

**Сервис-ориентированная архитектура** (SOA) - модульный подход к разработке программного обеспечения, основанный на использовании распределённых, слабо связанных (англ. loose coupling) заменяемых компонентов, оснащённых стандартизированными интерфейсами для взаимодействия по стандартизированным протоколам.

--

## Микросервисы vs Монолитная архитектура

--

## Аналогия с паттернами проектирования ПО

* Low Coupling (Слабое зацепление).
* High Cohesion (Сильная связность).

--

## Lamp


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

## Nginx + fpm + upstream

	FROM ubuntu:14.04
	RUN apt-get -y install ... php5-fpm nginx && ...
	ADD upstream /etc/nginx/common/upstream
	ADD default /etc/nginx/sites-available/default
	...

--

## Nginx + fpm + upstream

	upstream php-fpm
	{
		server unix:/var/run/php5-fpm.sock;
	}

	server {
    	listen 80;
    	...
    }

--

## fig - это ...

инструмент, который позволяет описывать запуск нескольких docker контейнеров, объединенных в одно приложение

Недавно влился в docker.

--

## Но, к сожалению 

Fig позволяет управлять контейнерами в рамках одной машины.

--

## Приложение на nodejs

* nodejs
* redis
* mongodb

--

**Dockerfile**

    FROM dockerfile/nodejs
    
    RUN git clone {repo} /app
    RUN cd /app && npm -g install grunt-cli && npm install

--

**fig.yml**


    redis:
        image: redis
    mongo:
        image: mongo        
    web:
        build: .
        links: 
            - redis
            - mongo

--

## Список команд

* `fig up` - запускает (перезапукает) сервисы
* `fig build` - запускает `docker build `
* `fig start` - запукает остановленные контейнеры
* `fig stop` - останавливаем контейнеры
* `fig rm` - удаляем остановленные контейнеры
* `fig ps` - список запущенных коонтейнеров
* `fig scale` - позволяет запускать несколько сервисов

--

## Ansible

Удаленное управление конфигурациями серверов.

--

## Почему он обсуждается здесь?

* Docker - стабильность, легкость, переносимость
* Ansible - процесс управления инфраструктурой

--

## Ansible

* Работает по принципу PUSH 
* Исполняет все команды на удаленных машинах через SSH

--

## Ansible

* inventory - список хостов и делим их по группам
* playbook - файл задач

--

## Ansible

- **Плейбуки** — исполняемый набор чего-угодно
- **Роли** - набор задач, шаблонов, тригеров-обработчиков, переменных, файлов 

--

## Плейбуки - это yml файлы


    - hosts: web
      sudo: yes
      tasks:
      - name: check or build image
        docker_image: path="/path/to/build" name="my/app" state=present


--

## Ansible

**Модули**. При помощи модулей мы можем выполнять различные операции на удаленном сервере или группе серверов. 

--


## Собственно, ...

* Имеет модуль для управления docker контейнерами 
* Имеет модуль для управления docker образами

--

## Что дальше?

<ul>
	<li class="next">Docker Machine</li>
	<li class="next">Docker Swarm</li>
	<li class="next">Docker Compose</li>
</ul>

--


## That's all, folks!

<span>Виталий Сверчков & Юрий Сафаргалиев</span>

<span>Tech Meetup: технические встречи АИР</span>

<a href="https://github.com/swvitaliy/air-docker-presentaion">github</a> | 
<a href="https://github.com/swvitaliy/docker-bochar">docker для запуска</a>

<span>Саранск, 2015</span>

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/swvitaliy/air-docker-presentaion/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

