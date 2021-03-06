


## Задача 1

Сценарий выполения задачи:

- создайте свой репозиторий на https://hub.docker.com;
- выберете любой образ, который содержит веб-сервер Nginx;
- создайте свой fork образа;
- реализуйте функциональность:
запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
```
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

## Ответ:
```
https://hub.docker.com/repository/docker/kav2891/netology_edu/
```
## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

- Высоконагруженное монолитное java веб-приложение;
- Nodejs веб-приложение;
- Мобильное приложение c версиями для Android и iOS;
- Шина данных на базе Apache Kafka;
- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
- Мониторинг-стек на базе Prometheus и Grafana;
- MongoDB, как основное хранилище данных для java-приложения;
- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

### Ответ:
```
Высоконагруженное монолитное java веб-приложение;
 - физический сервер, т.к. монолитное, следовательно в микросерверах не реализуемо без изменения кода,
   и так как высоконагруженное -  то необходим физический доступ к ресурсами, без использования гипервизора виртуалки. 
```
``` 
Nodejs веб-приложение;
 - это веб приложение, для таких приложений достаточно докера, противопоказаний для контейнерной реализации не вижу.
   и в рамках микропроцессрной архитектуры может быть хорошим решением. 
```
``` 
Мобильное приложение c версиями для Android и iOS;
 - Виртаулка -  приложение в докере не имеет GUI, а это по описанию не вариант. 
```
```
Шина данных на базе Apache Kafka;
 - зависит от передаваемых данных или контура (тест/прод), для прода и критичности данных лучше Вируалка, для теста достаточно Контейнерной реализации, если потеря данных при потере контейнера не является критичной то можно и в контейнере. 
```
```  
Elastic stack для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
 - сам Elasticsearvh лучше на виртуалку, отказоустойчивость решается на уровне кластера, 
   кибану и логсташ можно вынести в докер контейнер, или так же на виртуалках.
   существуют решения и в таком и в таком исполнении - в интернете есть описания (ссылки не стал кприкладывать :) )
   использование в Докерем может дать "+" для использования в тестовом периоде с максимальным функционалом, 
   в докере можно перезаливать машину каждый месяц и получать опять полную функциональность
```
```
Мониторинг-стек на базе prometheus и grafana;
 - сами системы не хранят как таковых данных, можно развернуть на Докере,
   минусов не вижу, в + скорость развертывания, возможность масштабирования для различных задачь: 
     например у графаны есть ограничение на число обрабатываемых метрик и может понадобиться разделения под разные задачи (контуры)
```
```
Mongodb, как основное хранилище данных для java-приложения;
 - можно использовать Виртуальную машину, т.к. хранилище и  не сказано что высоконагруженное
   в Контейнере хранить БД с данными не подходит
   для физического сервера может быть через чур расточительно
```

## Задача 3

- Запустите первый контейнер из образа ***centos*** c любым тэгом в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```;
- Добавьте еще один файл в папку ```/data``` на хостовой машине;
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.

### Ответ:
- Листинг из терминала контейнер CentOS
```
[root@8660ba467c81 data]# cat > /home/data/test.txt <<EOF
> Hello Netology!
> Test file!!!
> EOF
[root@8660ba467c81 data]# cat /home/data/test.txt
Hello Netology!
Test file!!!
[root@8660ba467c81 data]#
```

- Листинг из терминала контейнер Debian
```
root@vagrant:/home/vagrant# docker exec -it deb /bin/bash
root@d5be6392695a:/# cd home/data
root@d5be6392695a:/home/data# ls
test.txt
root@d5be6392695a:/home/data# cat test.txt
Hello Netology!
Test file!!!
root@d5be6392695a:/home/data#
```
- Листинг из терминала локального хоста.
```
root@vagrant:/home/vagrant/data# ls
test.txt
root@vagrant:/home/vagrant/data# cat test.txt
Hello Netology!
Test file!!!
```
