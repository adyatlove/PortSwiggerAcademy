Перейдем во вкладку с подарками, и в `Proxy` -> `HTTP history` найдем данный запрос, и отправим его в `Repeater`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/4.%20SQL%20injection%20UNION%20attack%2C%20finding%20a%20column%20containing%20text/pics%20for%20walktrough/1.png)

Далее нам необходимо узнать количество столбцов в таблице с товарами. Сделаем это по аналогии с предыдущей лабораторной работой. Запрос представлен сразу в URL кодировке
```
GET /filter?category=Corporate+gifts'+UNION+SELECT+NULL,NULL,NULL--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/4.%20SQL%20injection%20UNION%20attack%2C%20finding%20a%20column%20containing%20text/pics%20for%20walktrough/2.png)

Далее ищем какой из столбцов именно имеет тип поля `text`. Для этого вместо `NULL` на каждой из позиций введем любое текстовое значение (по заданию необходимо вводить `'t8n4xc'`, его и будем сразу вводить)
```
GET /filter?category=Corporate+gifts'+UNION+SELECT+'t8n4xc',NULL,NULL--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/4.%20SQL%20injection%20UNION%20attack%2C%20finding%20a%20column%20containing%20text/pics%20for%20walktrough/3.png)
Видим неудачу, переставим значение `'t8n4xc'` в соседний столбец
```
GET /filter?category=Corporate+gifts'+UNION+SELECT+NULL,'t8n4xc',NULL--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/4.%20SQL%20injection%20UNION%20attack%2C%20finding%20a%20column%20containing%20text/pics%20for%20walktrough/4.png)
Видим, что вернулся 200 респонс статус код. Это означает, что второй столбец в данной БД имеет текстовое значение. Лабораторная работа выполнена
