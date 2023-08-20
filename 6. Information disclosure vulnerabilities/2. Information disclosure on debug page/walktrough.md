Откроем уязвимый веб-ресурс
Просканим структуру сайта, используя `Target` -> `Site map` -> `Engagement tools` -> `Discover content`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/2.%20Information%20disclosure%20on%20debug%20page/pics%20for%20walktrough/1.png)

Замечаем директорию `/cgi-bin/phpinfo.php` которая может содержать важную информацию о системе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/2.%20Information%20disclosure%20on%20debug%20page/pics%20for%20walktrough/2.png)

Отправим ее в `Repeater` и выполним данный запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/2.%20Information%20disclosure%20on%20debug%20page/pics%20for%20walktrough/3.png)

Видим, что действительно отобразилась информация о версии php и множество других параметров. Найдем значение `SECRET_KEY`, которое нужно нам для выполнения задания
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/2.%20Information%20disclosure%20on%20debug%20page/pics%20for%20walktrough/4.png)

В режиме Render он не отобразит полную страницу, так что если Вам удобнее смотреть по отрисованной странице, а не по HTML разметке, то можно зайти по 
```
https://0a2900a203e2f8cc82b8388300d00022.web-security-academy.net/cgi-bin/phpinfo.php

```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/2.%20Information%20disclosure%20on%20debug%20page/pics%20for%20walktrough/5.png)

И получаем `SECRET_KEY = tzvb1pfone8q14y7jh5sfds9gbb4bnn2`, который мы и сдадим в качестве ответа на лабораторную работу
