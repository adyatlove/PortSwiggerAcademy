Зайдем на любую карточку товара, который нам приглянулся
Далее зайдем в `Proxy` -> `HTTP history` и найдем следующий запрос
'''
GET /product?productId=1
'''
Именно данный GET запрос и открывает нужную страницу с товаром
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/1.%20Information%20disclosure%20in%20error%20messages/pics%20for%20walktrough/1.png)
Далее, мы видим, что значение `productId=` имеет значение типа `integer`. Подадим на вход ему значение другого типа, к примеру string. 
Введем абсолютно любую строку в параметр `productId=`
```
GET /product?productId='non-integer-string'
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/1.%20Information%20disclosure%20in%20error%20messages/pics%20for%20walktrough/2.png)
Видим 500 статус код, который вывалил нам информацию об устройсте сервера, и по заданию нам необходимо найти версию Apache - `Apache Struts 2 2.3.31`

`2 2.3.31` Его и сдадим в качестве ответа на лабораторную работу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/1.%20Information%20disclosure%20in%20error%20messages/pics%20for%20walktrough/3.png)
