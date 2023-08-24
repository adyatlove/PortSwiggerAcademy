Откроем директорию `robots.txt`, которая создана для ботов-индексаторов, чтобы поисковые системы не выдавали определенные директории в общей выдаче
```
https://0ad1000703df293d831a833300230085.web-security-academy.net/robots.txt
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/1.%20Unprotected%20admin%20functionality/pics%20for%20walktrough/1.png)
Как мы и предполагали, владельцы веб-ресурса захотели скрыть директорию `/administrator-panel`. Посмотрим что там внутри

```
https://0ad1000703df293d831a833300230085.web-security-academy.net/administrator-panel
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/1.%20Unprotected%20admin%20functionality/pics%20for%20walktrough/2.png)
Мы получили доступ к админке, удалим там пользователя с именем `carlos`, как этого и требует задание
