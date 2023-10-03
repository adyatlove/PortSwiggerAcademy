Перейдем на уязвимый ресурс, и залогинимся под wiener:peter 

Далее перейдем на `https://www.revshells.com/` и выберем оттуда шелл на php и сохраним его в созданный в файловом хранилище с названием `exploit.php`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/2.%20Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/pics%20for%20walktrough/0.png)
На следующем этапе загрузим `exploit.php` в качесиве аватара для нашего аккаунта.
И видим `403 forbid`, запрещающий нам это сделать
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/2.%20Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/pics%20for%20walktrough/1.png)
Приложение само определяет тип загружаемого файла, и скорее всего имеет фильтр по полю `Content-type`. Давайте изменим значение поля Content-type с `application/x-php` на `image/jpeg` и проверим гипотезу

Видим, что шелл успешно загружен
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/2.%20Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/pics%20for%20walktrough/2.png)
Далее обратимся к нашему шеллу, чтобы убедиться в правильности пути
```
GET /files/avatars/exploit.php
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/2.%20Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/pics%20for%20walktrough/3.png)
Узнаем секрет УЗ `carlos`
```
GET /files/avatars/exploit.php?cmd=cat+'/home/carlos/secret'
```
и получаем в ответ  секрет
`TSycnNPrfBTY00inJNfmFovueblEnF5N`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/2.%20Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/pics%20for%20walktrough/4.png)

В качестве ответа на задание вводим полученный секрет и получаем сообщение об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/2.%20Web%20shell%20upload%20via%20Content-Type%20restriction%20bypass/pics%20for%20walktrough/5.png)
