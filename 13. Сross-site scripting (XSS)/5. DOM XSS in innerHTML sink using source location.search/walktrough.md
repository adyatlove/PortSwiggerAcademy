![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/5.%20DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search/pics%20for%20walktrough/1.png)
```
<img src=1 onerror=alert(1)>
```
Значение атрибута `src` недопустимо и выдает ошибку. Это запускает обработчик события onerror, который затем вызывает функцию alert(). В результате полезная нагрузка выполняется всякий раз, когда браузер пользователя пытается загрузить страницу, содержащую ваше вредоносное сообщение.

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/5.%20DOM%20XSS%20in%20innerHTML%20sink%20using%20source%20location.search/pics%20for%20walktrough/2.png)
