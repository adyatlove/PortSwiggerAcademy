Попытаемся залогиниться под любой связкой логин-пароль. И перейдем в `Proxy` -> `HTTP history` и найдем данный запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/1.png)
Отправим его в `Intruder` и выберем перебор по значению поля `username`
```
POST /login
...
username=§qwe§&password=123
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/2.png)
В раздел payloads добавляем логины, предлженные самим бурпом
```
https://portswigger.net/web-security/authentication/auth-lab-usernames
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/3.png)
В раздел `Settings` -> `Gpep - Match` добавим значение `Invalid username or password.`
И запустим атаку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/4.png)
После завершения атаки отфильтруем результат по полю `Invalid username or password.`, и заметим, что только один логин не подходит под выделенный фильтр, и имя этой учетной записи - `azureuser`  
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/5.png)
Далее перейдем к бруту пароля. Пароли так же возьмем из предложенных для выполнения лабораторной работы с бурп академии
```
https://portswigger.net/web-security/authentication/auth-lab-passwords
```
И на этот раз провдем атаку уже по значению поля `password`
```
POST /login
...
username=azureuser&password=§123§
```
и запустим атаку

Полученный результат остортируем по фильтру `Status code`, и видим, что только один пароль (`qazwsx`) вызывает за собой 302 статус код - редирект
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/6.png)

Перейдем на главную страницу и залогинимся под кредами `azureuser:qazwsx` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/2.%20Username%20enumeration%20via%20subtly%20different%20responses/pics%20for%20walktrough/7.png)
