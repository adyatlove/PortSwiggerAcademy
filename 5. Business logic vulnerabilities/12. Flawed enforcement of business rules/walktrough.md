Авторизуемся под пользователем `wiener:peter`
И заметим, что на корневой странице нам дали промокод - `NEWCUST5`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/12.%20Flawed%20enforcement%20of%20business%20rules/pics%20for%20walktrough/1.png)
Добавим в корзину нужный нам по заданию товар и применим данный промокод, и видим, что нам действительно дали скидку. Мелочь, а приятно
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/12.%20Flawed%20enforcement%20of%20business%20rules/pics%20for%20walktrough/2.png)
Далее вернеся на корневую страницу и прилистаем ее вниз. Замечаем форму для подписки на рассылку, где необходимо ввести свой e-mail. Введем туда абсолютно любой. Я ввел `a@a`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/12.%20Flawed%20enforcement%20of%20business%20rules/pics%20for%20walktrough/3.png)

Как результат, нам дали еще один промокод на скидку - `SIGNUP30`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/12.%20Flawed%20enforcement%20of%20business%20rules/pics%20for%20walktrough/4.png)

Далее применим промокод `SIGNUP30` в нашей корзине товаров. Видим, что он значительно больше предыдущего. Но, к сожалению, дважды его не применить, отрабаотывает блок.
Но если мы снова применим сертификат `NEWCUST5` - он сработает, и он активируется снова. Соответсвенно, логикой заложено, что нельзя активировать 2 одинаковых сертификата подряд. Чтож, будем чередовать `NEWCUST5` и `SIGNUP30` пока у нас не хватит денег на покупку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/12.%20Flawed%20enforcement%20of%20business%20rules/pics%20for%20walktrough/5.png)

Оформим заказ и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/12.%20Flawed%20enforcement%20of%20business%20rules/pics%20for%20walktrough/6.png)
