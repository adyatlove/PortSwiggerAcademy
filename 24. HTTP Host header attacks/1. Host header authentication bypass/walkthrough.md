Перейдем на уязвимый ресурс

Перейдем в `Target` -> `ПКМ` -> `Scan` -> `scan configuration` -> `fast`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/1.%20Host%20header%20authentication%20bypass/pics%20for%20walkthrough/1.png)

и по результатам сканирования увидим, что существует директория `/robots.txt`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/1.%20Host%20header%20authentication%20bypass/pics%20for%20walkthrough/2.png)

Перейдем по ней, и увидим, что для роботов-индексаторов существует и директория `/admin`, что скрыта от глаз многих

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/1.%20Host%20header%20authentication%20bypass/pics%20for%20walkthrough/3.png)

Перейдем по директории 
```
GET /admin
```

и увидим ответ `401 Unauthorized` 
и надпись `Admin interface only available to local users`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/1.%20Host%20header%20authentication%20bypass/pics%20for%20walkthrough/4.png)

Изменим header `host`
```
Host: localhost
```

и получим результат, где мы смогли обойти защиту приложения

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/1.%20Host%20header%20authentication%20bypass/pics%20for%20walkthrough/5.png)


отлично, теперь удалим пользователя carlos, как этого и требует задание

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/1.%20Host%20header%20authentication%20bypass/pics%20for%20walkthrough/6.png)
