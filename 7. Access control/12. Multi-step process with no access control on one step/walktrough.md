Залогинимся под УЗ `administrator:admin`
и попадем на страницу для повышения прав пользователей
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/12.%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/pics%20for%20walktrough/1.png)
И посмотрим через `Proxy` -> `HTTP history` полную цепочку запросов для повышения/понижения привилегий для конкретного пользователя с админской панели

Первый запрос для повышения
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/12.%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/pics%20for%20walktrough/2.png)

Далее идет второй, который нужно подтвердить, мол, чтобы админ не ошибся, и подтвердил сови действия
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/12.%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/pics%20for%20walktrough/3.png)

Оба данных запроса отправим в `Repeater`

Следующим шагом разлогинимся из УЗ administrator и зайдем в УЗ `wiener:peter`
и заберем Cookie его сессии
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/12.%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/pics%20for%20walktrough/4.png)
Далее в Repeater изменим запрос на повышение привилегий для конкретного пользователя. Сначала изменим в первом запросе с `user=carlos` на `user=wiener`, а так же изменим куки

```
POST /admin-roles HTTP/2
...
uK8iyuHCMMO4JDnmqy5AeJJb3iCIM51A
...
username=wiener&action=upgrade
...
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/12.%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/pics%20for%20walktrough/5.png)

Видим, что после первого запроса мы получили 401 код ошибки. Но в целом, нам это и не важно. Главное, чтобы прошел второй запрос, в котороы повторим аналогичные действия
```
POST /admin-roles HTTP/2
...
uK8iyuHCMMO4JDnmqy5AeJJb3iCIM51A
...
action=upgrade&confirmed=true&username=wiener
...
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/12.%20Multi-step%20process%20with%20no%20access%20control%20on%20one%20step/pics%20for%20walktrough/6.png)

После этого обновим страницу в браузере, и видим, что теперь УЗ `wiener` тоже обладает админскими правами в системе
