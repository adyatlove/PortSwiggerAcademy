Зайдем на уязвимый ресурс 

перейдем в `Proxy` -> `HTTP history` и проанализируем запросы

найдем запрос и отправим его в `Repeater`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/1.png)

Нажмем на него `ПКМ -> Extensions -> Param miner -> Guess Headers` и попробуем найти уязвимые заголовки, которые нам могут помочь
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/2.png)

Спустя время в `Target` -> `issues` увидим, что мы нашли уязвимый заголовок - `x-forwarded-scheme`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/3.png)

Вернемя в Repeater и введем
```
X-Forwarded-Scheme: http
```
и увидим, что в ответе появилсся заголовок 
```
Location: https://0a1e000304082c8181ae9e82005a00d8.web-security-academy.net/resources/js/tracking.js
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/4.png)

Снова нажимем `ПКМ -> Extensions -> Param miner -> Guess Headers` и попробуем найти уязвимые заголовки, которые нам могут помочь

Спустя время в `Target` -> `issues` увидим, что мы нашли уязвимый заголовок - `x-forwarded-host`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/5.png)

добавим заголовок 
```
X-Forwarded-Host: expample.com
```
и увидим в ответе, что нас редиректит на 
```
Location: https://expample.com/resources/js/tracking.js?cb=123
```
отлично, нам это и нужно
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/6.png)

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/7.png)

Далее перейдем в `Exploit server` и подготовим страничку, на которую будет редиректить пользователей
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/8.png)

Веренмся в Repeater и в заголовок X-Forwarded-Host введем на эксплойт хост `exploit-0a01003504102c7c81799dae01170054.exploit-server.net`

Закешируем запрос и ждем пока пользователь воспользуется кешированным запросом и получим уведомление об успешной атаке

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/3.%20Web%20cache%20poisoning%20with%20multiple%20headers/pics%20for%20walkthrough/9.png)
