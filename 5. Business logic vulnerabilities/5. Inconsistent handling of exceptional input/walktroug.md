После загрузки корневой страницы попробуем просканировать данный ресурс на имеюющиеся в нем директории
Для этого перейдем в `Target` -> `Site map` -> `Engagement tools` -> `Discover content`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/1.png)
Запустим его, и зайдем во вкладку `Site map`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/2.png)

Мы с Вами нашли директорию `/admin`. Зайдем на нее, изменим URL адрес на
```
https://0aab007f03c94101842deb670025005d.web-security-academy.net/admin
```
Перейдя по ссылке, видим сообщение, что админская панель доступна только DontWannaCry user
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/3.png)

Давайте проверим как работает авторизация, и есть ли уязвимости в авторизации.
Для этого зарегистрируем пользователя `user1` с очень длинным адресом e-mail. В нашем случае, адрес e-mail будет
```
very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string@exploit-0a820098039e41728498ea55013400c9.exploit-server.net
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/4.png)
Зайдем на почтовый ящик, и подтвердим регистрацию
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/5.png)
Перейдем в информацию о пользователе, и увидим, что ресурс обрезал часть нашей почты.
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/6.png)

Разлогинимся и создадим `user3` и добавим в почтовый адрес домен `dontwannacry.com`, чтобы при обрезании нашего почтового ящика получилось нечто вроде `(email)@dontwannacry.com(а оставшаяся тут часть обрежется)`

И полный email будет формата 
```
1very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string-very-long-string@dontwannacry.com.exploit-0a820098039e41728498ea55013400c9.exploit-server.net

```
Видим, что у нас появилась админская панель, в которую мы и зайдем
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/8.png)


По заданию необходимо удалить пользователя `carlos`, что мы и сделаем
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/5.%20Inconsistent%20handling%20of%20exceptional%20input/pics%20for%20walktrough/9.png)

В ответ получим сообщение об успешно выполненной лабораторной работы
