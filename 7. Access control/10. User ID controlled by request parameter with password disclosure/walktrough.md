Залогинимся под пользователем `wiener:peter` и узнаем информацию о своем аккаунте
```
https://0a16004403628eb38004c62b00bc00d8.web-security-academy.net/my-account?id=wiener
```
Явно указывающий в URL параметр `id=...` дает нам надежду на боковое перемещение между пользователями. 
Найдем данный запрос в `Proxy` -> `HTTP history`, и отправим данный запрос в `Repeater`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/10.%20User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/pics%20for%20walktrough/1.png)

Так же заметим, что пароль указывается в HTML разметке в чистом виде. Да, для обычного пользователя он скрыт спец. символом, но от HTML разметки этого не скрыть.

Изменим параметр `id=wiener` на `id=administrator`
```
GET /my-account?id=administrator
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/10.%20User%20ID%20controlled%20by%20request%20parameter%20with%20password%20disclosure/pics%20for%20walktrough/2.png)

И как видим 200 ответ, и так же замечаем, что нам не просто выдало страницу пользователя, а вместе с ним еще и его пароль `q3jv3zfc10vakveysao3`

Далее залогинимся под `administrator:q3jv3zfc10vakveysao3` и удалим УЗ `carlos`, как это и требует задание
