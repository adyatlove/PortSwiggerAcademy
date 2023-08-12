После загрузки корневой страницы попробуем просканировать данный ресурс на имеюющиеся в нем директории
Для этого перейдем в `Target` -> `Site map` -> `Engagement tools` -> `Discover content`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/1.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/2.png)
Мы с Вами нашли директорию `/admin`. Зайдем на нее, изменим URL адрес на
```
https://0afc00a004ad82ad8383dc71005600ef.web-security-academy.net/admin
```
Перейдя по ссылке, видим сообщение, что существует особая категория людей с доменом `DontWannaCry`
Зарегистрируемся используя наш почтовый ящик, который нам предоставили, введя e-mail `user1@exploit-0abf0066044482f48391db4201cf00ff.exploit-server.net`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/4.png)

Перейдем в почтовый ящик и перейдем по ссылке для подтверждения почты
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/3.png)

Далее вспомним, что есть особый домен `DontWannaCry`. Изменим нашу почту на почту с доменом `@dontwannacry.com`, соответсвенно, наша почта будет выглядеть как
```
user1@dontwannacry.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/5.png)

Отлично, видим что почта успешно изменилась и без всякого подтверждения, и так же у нас появилась админская панель, в которую, мы, конечно же, зайдем
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/6.png)

Удалим пользователя `carlos`, как это требует задание
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/6.%20Inconsistent%20security%20controls/pics%20for%20walktrough/7.png)

В результате получим сообщение `User deleted successfully!` и сообщение об успешно выполненной лабораторной работе!
