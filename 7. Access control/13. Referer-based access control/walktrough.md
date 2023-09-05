Залогимся под `Administrator:admin` и перейдем в админскую панель

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/13.%20Referer-based%20access%20control/pics%20for%20walktrough/1.png)
Добавим пользователю `carlos` прав, и посмотрим, какой запрос вызвало данное действие
```
GET /admin-roles?username=carlos&action=upgrade
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/13.%20Referer-based%20access%20control/pics%20for%20walktrough/2.png)
Отправим его в `Repeater`
Далее разлогинимся и залогинимся под пользователем `wiener:peter`, и найдем Cookie УЗ `wiener`
```
8CfZzCOqc2CAChCVISwFtKwCW4NfOglh
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/13.%20Referer-based%20access%20control/pics%20for%20walktrough/3.png)

Далее, изменим `username=carlos` на `username=wiener` и подставив Cookie `wiener` в запрос получаем
```
GET /admin-roles?username=wiener&action=upgrade
...
Cookie: session=8CfZzCOqc2CAChCVISwFtKwCW4NfOglh
...
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/13.%20Referer-based%20access%20control/pics%20for%20walktrough/4.png)
Веб сервер принял запрос, и повысил привилегии пользователя `wiener` не из админской учетной записи
