введем в поисковую строку qwe123
```
<img src="/resources/images/tracker.gif?searchTerms=qwe123">
```
и посмотрим код страницы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/18.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20HTML%20encoded/pics%20for%20walkthrough/1.png)
попробуем выйти из данного тэга
введем `'-alert(1)-'`
и получим уведомление об успешно выполненной лаборатной работе
