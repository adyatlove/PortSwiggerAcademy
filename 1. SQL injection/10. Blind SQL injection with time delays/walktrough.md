Перейдем на уязвимый (по условию задания) ресурс, и в `Proxy` -> `HTTP history` найдем запрос, который отроем нам корневую страницу, и отрпавим ее в `Repeater`
```
GET /
Cookie: TrackingId=jC8ihFCHeQQ5tTHZ;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/10.%20Blind%20SQL%20injection%20with%20time%20delays/pics%20for%20walktrough/1.png)
По заданию, приложение содержит слепую SQL-инъекцию в куки. Чтобы выполнить задание необходимо вызвать временную задержку от базы данных, и если эта временая задержка будет существовать - это будет нашим тригером для слепой инъекции

Авторы предлагают нам читлист с примерами для различных баз данных, и как вызвать временную задержку в каждой из них
```
https://portswigger.net/web-security/sql-injection/cheat-sheet
```
Последовательно пробуем каждую из них, и получаем успешный временную задержку от следующего запроса 
```
Cookie: TrackingId=jC8ihFCHeQQ5tTHZ'|| (SELECT pg_sleep(10))--;
```
Что означает, что работаем мы с `PostgreSQL`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/10.%20Blind%20SQL%20injection%20with%20time%20delays/pics%20for%20walktrough/2.png)
Обновим страницу и увидим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/10.%20Blind%20SQL%20injection%20with%20time%20delays/pics%20for%20walktrough/3.png)
