### Задача 1
Сценарий выполения задачи:

создайте свой репозиторий на https://hub.docker.com;

выберете любой образ, который содержит веб-сервер Nginx;

создайте свой fork образа;

реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:

<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на  
https://hub.docker.com/username_repo  


### Решение:
https://hub.docker.com/repository/docker/antkonyakhin/docker-nginx


### Задача 2
Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

1. Высоконагруженное монолитное java веб-приложение;  
Так приложение высоконагруженное, считаю, что здесь лучше подойжет физический хост, исключая слои виртуализации для надежности
2. Nodejs веб-приложение;  
Хорошо подойдет для работы в контейнере docker  
3. Мобильное приложение c версиями для Android и iOS;  
для IOS есть Docker Desktop for Mac, для Android нет, нужно использовать эмулятор  

4. Шина данных на базе Apache Kafka;  
подойдет Docker, есть образ https://hub.docker.com/r/bitnami/kafka/  

5. Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;  
Подойдет Docker, есть информация на сайте https://www.docker.elastic.co/  

6. Мониторинг-стек на базе Prometheus и Grafana;  
для Prometheus подойдет Docker, есть документация https://prometheus.io/docs/prometheus/latest/installation/  
для Grafana подойдет Docker, есть документация https://grafana.com/docs/grafana/latest/installation/docker/  

7. MongoDB, как основное хранилище данных для java-приложения;  
подойдет docker, в который подключен volume  
https://hub.docker.com/_/mongo  

8. Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.  
подойдет Docker, есть документация https://docs.gitlab.com/ee/install/docker.html  

### Задача 3
- Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;  
```
vagrant@server1:~/docker/docker-vol$ docker run -dt -v $PWD/data:/data --name cent-2 centos
023e57a311bb4783c91d4023a8bbee344e5b2685d68083d88c441c56af10e389

```
- Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;  
```
vagrant@server1:~/docker/docker-vol$ docker run -dt -v $PWD/data:/data --name deb-2 debian
6eb721d1b638fa274a54657bd1cbd52cf1d534623325944efc1f892cb5ea8cfe
```
- Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;  

```
vagrant@server1:~/docker/docker-vol$ docker exec cent-2 bash -c 'echo "Hello from container centos">/data/file1'
```

- Добавьте еще один файл в папку /data на хостовой машине;
```
vagrant@server1:~/docker/docker-vol$ echo "Hello from host">data/file2
```

- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.
запущенные контейнеры
```
agrant@server1:~/docker/docker-vol$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED             STATUS             PORTS     NAMES
6eb721d1b638   debian    "bash"        15 minutes ago      Up 15 minutes                deb-2
023e57a311bb   centos    "/bin/bash"   16 minutes ago      Up 16 minutes                cent-2
126c97b86cd1   debian    "bash"        24 minutes ago      Up 24 minutes                deb-1
c96226c0589b   centos    "/bin/bash"   About an hour ago   Up About an hour             cent-1

```
подключаемся к котейнеру по имени в интерактивном режиме
```
vagrant@server1:~/docker/docker-vol$ docker exec -it  deb-2 bash
```

выводжим содержимое подключенного volume
```
root@6eb721d1b638:/# ls -la /data/
total 16
drwxrwxrwx 2 1000 1000 4096 Feb 13 18:36 .
drwxr-xr-x 1 root root 4096 Feb 13 18:28 ..
-rwxrwxrwx 1 root root   28 Feb 13 18:35 file1
-rw-rw-r-- 1 1000 1000   16 Feb 13 18:36 file2
```
выводим содержимое файлов
```
root@6eb721d1b638:/# cat /data/file1 
Hello from container centos
root@6eb721d1b638:/# cat /data/file2 
Hello from host
root@6eb721d1b638:/# 

```