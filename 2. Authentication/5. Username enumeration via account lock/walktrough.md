Попробуем залогиниться под случайной связкой логина-пароля на предлагаемом сайте
Найдем данный запрос в `Proxy` -> `HTTP history` и отправим данный запрос в `Intruder`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/1.png)
Выберем тип атаки `Cluster bomb` и будем брутить по значению поля `username`, а так же поставим два подряд идущих спецсимвола в конце пэйлоада
```
POST /login
...
username=§qwe§&password=123§§
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/2.png)
В качестве первого пэйлоада выбырем `Simple list`, который означает брутфорс по четкому списку логинов, предложенных нам авторами лабораторной работы
```
https://portswigger.net/web-security/authentication/auth-lab-usernames
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/3.png)
В качестве второго пэйлоада выберем `Null payloads` и установим счетчик генерируемых пэйлоадов в 5
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/4.png)
Запустим атаку, и после ее завершения отсортируем ответы по длинне возвращаемой страницы, и видим логин - `arizona`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/5.png)
Далее, после того как мы выяснили логин, который существует в приложении, набрутим к нему пароль, для этого выберем атаку `Sniper` и будем брутить по значению поля `password`
```
POST /login
...
username=arizona&password=§123§
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/6.png)
Далее перейдем в раздел `Settings` -> `Grep - Extract` -> `Add` и выберем положение фразы `Invadid username or password`, чтобы мы могли найти успешную аутентификацию
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/7.png)
В разделе Payload выберем `Simple list` и в него вставим пароли, предлагаемые авторами лабораторной работы
```
https://portswigger.net/web-security/authentication/auth-lab-passwords
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/8.png)
Запустим атаку

После ее завершения отфильтруем по полю `Invadid username or password` и получим, что пароль - `buster`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/9.png)
Залогинимся под кредами `arizona:buster` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/5.%20Username%20enumeration%20via%20account%20lock/pics%20for%20walktrough/10.png)
