Зайдем в `Live chat`, и отравим несколько сообщений
Далее скачаем нашу переписку используя кнопку `View transcript`. Заметим, что скачался файл 2.txt Значит, нам нужно поискать файл 1.txt. 

Найдем данный `GET` запрос на скачивание переписки в `Proxy` -> `HTTP history`
```
GET /download-transcript/2.txt
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/11.%20Insecure%20direct%20object%20references/pics%20for%20walktrough/1.png)

Изменим данный `GET` запрос на 
```
GET /download-transcript/1.txt
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/11.%20Insecure%20direct%20object%20references/pics%20for%20walktrough/2.png)

Видим переписку прошлого пользователя, в котором мелькают пароли
```
...
Ok so my password is pc0z503ay4a78iaq9jfu
...
```
На финальном моменте, зная пароль, и пользователя, которого нам дали по заданию, аутентифицируемся под `carlos:pc0z503ay4a78iaq9jfu` и получим уведомление об успешно сделанной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/11.%20Insecure%20direct%20object%20references/pics%20for%20walktrough/3.png)
