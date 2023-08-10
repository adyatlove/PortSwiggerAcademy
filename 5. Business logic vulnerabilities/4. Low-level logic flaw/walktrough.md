Авторизуемся под `wiener:peter` и перейдем на страницу покупки необходимого товара
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/1.png)
Добавим его в корзину и отправим данный `POST` запрос в `Intruder`
```
POST /cart
...
productId=1&redir=PRODUCT&quantity=1
```
И выберем `payload type - Null payloads`. Но запускать пока не будем
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/2.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/3.png)
Перед запускам удалим уже имеющиеся покупки, чтобы корзина была пустой
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/4.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/5.png)
Далее запустим атаку в `Intruder`'е
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/6.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/9.png)
Несколько раз обновим страницу, пока не увидим отрицательное число в стоимости заказа
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/8.png)
Далее по аналогии через `Intruder`, добавляем любой товар и покупаем его столько, чтобы общая сумма заказа укладывалась в имеющиеся 100$
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/11.png)
Оформляем заказ и видим, что сервером он принят
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/4.%20Low-level%20logic%20flaw/pics%20for%20walktrough/12.png)
