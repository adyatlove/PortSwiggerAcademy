Перейдем на уязвимый ресурс 

перейдем в `Proxy` -> `HTTP history` и изучим запросы

увидим, что при запросе корневой страницы увидим заголовки
```
Cache-Control: max-age=35
Age: 2
X-Cache: hit
```
которые могут свидетельствовать о наличии уязвимости кеша
и отправим данный запрос в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/5.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/pics%20for%20walkthrough/1.png)


Далее увидим, что при отправке запросов которые не являются в кеше - то есть когда у нас 

`X-Cache: miss` мы видим, что путь отображается и на главной странице

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/5.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/pics%20for%20walkthrough/2.png)

Попробуем выйти за ковычки, применив XSS уязвимость

Снова отправляем до тех пор, пока не будет `X-Cache: miss`

и увидим, что мы вышли за ковычки и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/23.%20Web%20cache%20poisoning/5.%20Web%20cache%20poisoning%20via%20an%20unkeyed%20query%20string/pics%20for%20walkthrough/3.png)
