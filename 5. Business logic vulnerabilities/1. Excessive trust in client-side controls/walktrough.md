Откроем потенциально уязвимый веб ресурс
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/1.%20Excessive%20trust%20in%20client-side%20controls/pics%20for%20walktrough/1.png)

Согласно заданию, нам дали логин и пароль для входа на сайт, введем их `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/1.%20Excessive%20trust%20in%20client-side%20controls/pics%20for%20walktrough/3.png)
Вау, 100 баксов, приятно.

Далее перейдем на страницу товара, которую нам нужно купить по заданию. К сожалению, сразу наблюдаем, что денег слегка не хватает, но ничего, попробуем это обойти
```
GET /product?productId=1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/1.%20Excessive%20trust%20in%20client-side%20controls/pics%20for%20walktrough/2.png)

Добавим в корзину понравившийся товар
```
POST /cart HTTP/2
...
productId=1&redir=PRODUCT&quantity=1&price=133700
```

Более того, изменим наш POST запрос, чтобы снизить стоимость товара. Да и чего мелочиться, возьмем 2
```
POST /cart HTTP/2
...
productId=1&redir=PRODUCT&quantity=2&price=100
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/1.%20Excessive%20trust%20in%20client-side%20controls/pics%20for%20walktrough/4.png)

Нажмем `Place order` чтобы разместить заказ
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/1.%20Excessive%20trust%20in%20client-side%20controls/pics%20for%20walktrough/5.png)

Поздравляю с успешно выполненной лабораторной работой!
