Откроем страницу случайного товара, и проверим его наличие через кнопку `Get stock`, и перейдем в `Proxy` -> `HTTP history` и найдем данный запрос
```
GET /product?productId=1
```
и отправим данный запрос в Repeater
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/6.%20Blind%20SSRF%20with%20out-of-band%20detection/pics%20for%20walktrough/1.png)
Найдем поле `Refferer` и заменим его значение на DGA домен коллаборатора нажав на `Insert Collaborator Payload`
```
Referer: https://mg6xz1p80go5y1gbojyujwsex53wrmfb.oastify.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/6.%20Blind%20SSRF%20with%20out-of-band%20detection/pics%20for%20walktrough/2.png)
отправим данный запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/6.%20Blind%20SSRF%20with%20out-of-band%20detection/pics%20for%20walktrough/3.png)
Далее перейдем на вкладку `Collaborator` и увидим, что прилетел отстук на наш DGA домен, а значит мы успешно выполнили задание лабораторной работы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/6.%20Blind%20SSRF%20with%20out-of-band%20detection/pics%20for%20walktrough/4.png)
