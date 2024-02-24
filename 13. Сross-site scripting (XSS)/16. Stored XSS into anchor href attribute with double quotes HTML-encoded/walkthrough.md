постим рандомный коммент 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/16.%20Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/pics%20for%20walkthrough/1.png)
выберем наш коммент, и нажмем `inspect` 

и увидим, что ссылка на наш сайт располагается в `href`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/16.%20Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/pics%20for%20walkthrough/2.png)
изменим ссылку на сайт как 
```
javascript:alert(1)
```
и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/16.%20Stored%20XSS%20into%20anchor%20href%20attribute%20with%20double%20quotes%20HTML-encoded/pics%20for%20walkthrough/3.png)
