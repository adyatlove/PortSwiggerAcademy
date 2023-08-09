Авторизуемся под пользователем `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/1.png)

Код для двухфакторной аутентификации приходит на почту
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/5.png)

Введем его, и полностью зайдем под данным пользователем
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/6.png)

В историях запросов найдем POST запрос для аутентификации с помощью двухфакторки
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/9.png)

Отправим данный запрос в `Intruder`
В нем мы изменим `wiener` на `carlos` и выберем перебор по кодам аутентификации по значению поля `mfa_code`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/8.png)

Настроим перебор по значению поля `mfa_code`:
Выберем `Payload type : Brute forcer` а так же `character set: 0123456789`
и запустим перебор
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/7.png)

Отсортируем результаты по `Status code` и увидим, что код для `carlos` равен `1481`
Отправим полученный запрос в `Repeater` и откроем его в браузере
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/11.png)

Когда зайдем через `Repeater`, нас приветсвует аккаунт `carlos` и сообщение о выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/3.%202FA%20broken%20logic/pics%20for%20walktrough/12.png)
