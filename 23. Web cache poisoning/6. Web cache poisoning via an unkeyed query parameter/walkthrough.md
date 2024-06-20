Перейдем на уязвимый ресурс

Перейдем в `Proxy` -> `HTTP history` и изучим запросы, найдем запрос на получение корневой страницы и отравим его в Repeater
тот, у которого есть параметр `Upgrade-Insecure-Requests: 1` !!!
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/6.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/pics%20for%20walkthrough/1.png)

Далее перейдем в `ПКМ` -> `Extensions` -> `Param miner` -> `Guess params` -> `Guess GET parameters`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/6.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/pics%20for%20walkthrough/2.png)

и подождем

Далее перейдем в `Target` -> `Issues` 
и увидим, что мы насканили параметр - `utm_content`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/6.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/pics%20for%20walkthrough/3.png)

Отлично, вернемся в Repeater и модифицируем запрос

```
GET /?cb=123
```
и увидим, что часть запроса отображается на странице

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/6.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/pics%20for%20walkthrough/4.png)
Попробуем выйти за кавычки
```
GET /?cb=123'/><script>alert(1)</script>
```
но это все равно не отрабатывает

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/6.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/pics%20for%20walkthrough/6.png)

Далее попробуем использовать параметр 
`utm_content`

```
GET /?utm_content='/><script>alert(1)</script>
```
и увидим, что в куки появляется хэдер `Set-Cookie: utm_content=%27%2f%3e%3cscript%3ealert%281%29%3c%2fscript%3e;` который и обеспечивает нам Reflected XSS
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/6.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20parameter/pics%20for%20walkthrough/5.png)
