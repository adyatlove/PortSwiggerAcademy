Залогинимся под `wiener:peter` и посмотрим в `Proxy` -> `HTTP history` полную цепочку запросов для аутентификации
Найдем запрос 
```
GET /my-account?id=wiener
...
Cookie: Admin=false;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/3.%20User%20role%20controlled%20by%20request%20parameter/pics%20for%20walktrough/1.png)

Далее, во всех запросах изменим запрос Cookie: `Admin=false` на `Cookie: Admin=true` 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/3.%20User%20role%20controlled%20by%20request%20parameter/pics%20for%20walktrough/3.png)
Лучше делать это в режиме `Proxy` -> `Intercept`
И видим, что мы добрались до админской панели
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/3.%20User%20role%20controlled%20by%20request%20parameter/pics%20for%20walktrough/2.png)

Удалим пользователя `carlos`, как этого и требует задание
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/3.%20User%20role%20controlled%20by%20request%20parameter/pics%20for%20walktrough/4.png)
