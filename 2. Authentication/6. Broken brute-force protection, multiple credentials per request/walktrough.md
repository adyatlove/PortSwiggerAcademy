Зайдем на уязвимый ресурс, и попытаемся залогиниться под случайными кредами
Найдем данный запрос в `Proxy` -> `HTTP history` и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/6.%20Broken%20brute-force%20protection%2C%20multiple%20credentials%20per%20request/pics%20for%20walktrough/1.png)
Модифируем запрос, сделав массив из паролей, и посмотрим, будет ли сервер обрабатывать каждый пароль из массива отдельно, или выдаст ошибку
```
password:["1234","1"]
```
И видим, что сервер отработал данный запрос и выдал 200 статус код. Разумеется, пароль не принят, но нас это не остановит!
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/6.%20Broken%20brute-force%20protection%2C%20multiple%20credentials%20per%20request/pics%20for%20walktrough/2.png)
Далее, возьмем большой список паролей, предложенных авторами лабораторной работы, и преобразуем из в массив паролей, как это было сделано выше
```
https://portswigger.net/web-security/authentication/auth-lab-passwords
```
Как результат, сервер выдал нам `302 респонс статус код`. Откроем его в оригинальной сессии браузера, и узнаем, куда он нас редиректит
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/6.%20Broken%20brute-force%20protection%2C%20multiple%20credentials%20per%20request/pics%20for%20walktrough/4.png)
А редиректит он на личную страницу УЗ `carlos`, к которой мы успешно подобрали пароль. И получили уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/6.%20Broken%20brute-force%20protection%2C%20multiple%20credentials%20per%20request/pics%20for%20walktrough/5.png)
