залогинимся под `wiener:peter`
И перейдем по директории админской панели
```
GET /admin
```
И видим что у нас недостаточно прав для просмотра данной страницы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/4.%20Authentication%20bypass%20via%20information%20disclosure/pics%20for%20walktrough/1.png)

Используем метод TRACE для получения дополнительной информации о доставляемых пакетах
```
TRACE /admin
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/4.%20Authentication%20bypass%20via%20information%20disclosure/pics%20for%20walktrough/2.png)
Обратим внимание, что заголовок `X-Custom-IP-Authorization`, содержащий наш IP-адрес, был автоматически добавлен к вашему запросу. Он обычно используется для определения того, пришел ли запрос с IP-адреса локального хоста.
Изменим данный параметр через настройки Burp Suite, чтобы обойти проверку
Перейдем в `Proxy` -> `options`, проскролим до `Match and replace rules` и выберем `Replace`
и впишем
```
X-Custom-IP-Authorization: 127.0.0.1
```
Сохраним и перейдем в по корневому каталогу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/4.%20Authentication%20bypass%20via%20information%20disclosure/pics%20for%20walktrough/3.png)
далее перейдем в браузере по корневому каталогу 
```
https://0aca00ee03d455d481aebba900b70039.web-security-academy.net/
```
и видим, что нам стала доступна админская панель, где мы удаляем карлоса
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/4.%20Authentication%20bypass%20via%20information%20disclosure/pics%20for%20walktrough/4.png)

И не забудьте удалить созданное матч энд реплейс! оно может помешать вам в следующих работах!
