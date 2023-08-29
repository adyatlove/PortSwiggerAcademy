Залогинимся под пользователем `administrator:admin`. И перейдем в саму админскую панель
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/6.%20Method-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/1.png)
Далее поднимем привилегии у УЗ carlos, и увидим, что это вызвало следующий запрос (смотрим в Proxy -> HTTP history)

```
POST /admin-roles
...
Cookie: session=Hw6CYVYUg5Ncz1g5NsmVd2vDZ1pO721w
...
username=carlos&action=upgrade
```
И отправим данный реквест в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/6.%20Method-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/2.png)
Разлогинимся, и теперь зайдем под УЗ `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/6.%20Method-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/3.png)
Замечаем, что админкой тут и не пахнет.
Модифицируем наш прошлый реквест (что находится в `repeater`), и изменим у него `Cookie` принадежащего УЗ `wiener`, а так же параметр `username=wiener&action=upgrade`
```
POST /admin-roles
...
Cookie: session=dMKUuUETvfkRov32Q0xpshW7d5AgxS6y
...
username=wiener&action=upgrade
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/6.%20Method-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/4.png)

Видим, что нам отдало 401 ответ, который нам не позволил таким образом повысить привилегии. Изменим запрос, нажав `ПКМ` -> `change request method`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/6.%20Method-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/5.png)

И отправим данный запрос на сервер
```
GET /admin-roles?username=wiener&action=upgrade
...
Cookie: session=dMKUuUETvfkRov32Q0xpshW7d5AgxS6y
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/6.%20Method-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/6.png)
Заметим, что сервер обработал запрос, и повысил привилегии пользователя `wiener` не из админской панели. Для завершения лабораторной работы вновь зайдите на корневую страницу
