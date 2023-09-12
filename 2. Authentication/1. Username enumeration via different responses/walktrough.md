Найдем форму аутентификации и попробуем аутентифицироваться под любым пользователем.
Далее в `Proxy` -> `HTTP history` найдем сам запрос и отправим его в `Intruder`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/1.png)


Для перебора в `Intruder` пометим значение поля `username` символами `§` с двух сторон
```
username=§qwe§&password=123
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/2.png)
Во вкладке Payloads вставим список возможных логинов из предложенных ресурсом. Пароль так и оставим рандомным
```
https://portswigger.net/web-security/authentication/auth-lab-usernames - список логинов
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/3.png)
Далее запустим атаку, и дождемся ее выполнения
Полученные результаты отсортируем по `Length`, и заметим, что у логина `test` сервер выдал надпись `incorrect password`, что в свою очередь означает, что УЗ `test` существует
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/4.png)

Далее повторим аналогичные действия, но уже по перебору пароля пользователя `test`
```
username=test&password=§123§
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/5.png)
Список возможных паролей найдем в предложенных ресурсом для обучения
```
https://portswigger.net/web-security/authentication/auth-lab-passwords
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/6.png)
Полученный результат можно отсортировкать как по `status code`, так и по `Length`. Видим, что именно пароль `666666` выделяется из общего списка
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/7.png)
Далее залогинимся под пользователем `test:666666` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/1.%20Username%20enumeration%20via%20different%20responses/pics%20for%20walktrough/8.png)
