Авторизуемся под пользователем `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/1.png)

Видим, что нам досутпна сумма в `100$`. Зайдем в магазин и добавим в корзину что-нибудь, что мы можем себе позволить
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/3.png)

И купим это
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/4.png)

Далее зайдем в `Proxy` -> `HTTP history` и посмотрим какие запросы были выполнены для совершения покупки
видим 2 запроса
`POST /cart/checkout` которому в ответ идет редирект
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/5.png)

а за ним идет `GET /cart/order-confirmation?order-confirmed=true` (и поместим его в `Repeater`)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/6.png)

Теперь же перейдем в магазин, и добавим в корзину нужный нам по заданию товар
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/7.png)

Далее мы НЕ будем нажимать на кнопку `Place order`, а минуя его, сразу отправим на веб-ресурс запрос, который мы поместили в `Repeater` (`GET /cart/order-confirmation?order-confirmed=true`), чтобы обойти проверку на баланс, и сразу перейти к одобрению покупки
И в респонсе получим уведомление об успешной покупке
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/10.%20Insufficient%20workflow%20validation/pics%20for%20walktrough/8.png)
