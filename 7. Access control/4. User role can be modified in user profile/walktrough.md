По заданию известно, что на сайте есть директория `/admin`, которая доступна только для пользователей с параметром `roleid:2`
Залогинимся под пользователем wiener:peter и проверим, доступна ли нам директория /admin
```
https://0ab900570307da5d84846812005d0042.web-security-academy.net/admin
```
И видим, что оно закрыто для нашего пользователя
На сайте присутствует функционал смены почты для учетной записи, проверим его. Сменим почту на `wiener@custom.net`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/4.%20User%20role%20can%20be%20modified%20in%20user%20profile/pics%20for%20walktrough/1.png)
В ответе видим, что у нас `roleid:1` 
Заметим, что в запрос входит json, который передает на какую именно почту нужно сменить нынешнюю. Добавим в данный json параметр `roleid:2`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/4.%20User%20role%20can%20be%20modified%20in%20user%20profile/pics%20for%20walktrough/2.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/4.%20User%20role%20can%20be%20modified%20in%20user%20profile/pics%20for%20walktrough/3.png)

Видим, что сервер обработал запрос, и изменил не только почту, но и изменил параметр roleid на необходимый нам
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/4.%20User%20role%20can%20be%20modified%20in%20user%20profile/pics%20for%20walktrough/4.png)
Далее зайдем в появившуюся админускую панель, и удалим УЗ `carlos`, как это и было сказано сделать по заданию
