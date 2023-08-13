Для начала изучим, как работает процесс по смене пароля
Нажмем `Forgot password`, и попробуем восстановить пароль для учетной записи `wiener`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/2.png)

Нам отправили уникальную ссылку на почту, для подтверждения смены пароля
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/1.png) 

Данная ссылка перебрасывает на страницу, где мы вводим новый пароль и повторяем его
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/7.png)

Найдем данный запрос в `Proxy` -> `HTTP history` и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/3.png)

Заменим учетную запись `wiener` на `carlos`, и установим для него совершенно другой пароль. Я решил установить самый стойкий из всех стойких - `1q2w3e4r5t`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/4.png)

Далее попытаемся авторизоваться под пользователем `carlos` с паролем `1q2w3e4r5t`, и видим, что у нас это получилось

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/6.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/7.%20Password%20reset%20broken%20logic/pics%20for%20walktrough/5.png)
