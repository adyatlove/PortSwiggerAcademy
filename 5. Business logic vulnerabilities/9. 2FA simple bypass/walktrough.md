Авторизуемся под данным нам юзером `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/9.%202FA%20simple%20bypass/pics%20for%20walktrough/1.png)

Зайдем во временный почтовый ящик для получение кода для двухфакторной аутентификации
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/9.%202FA%20simple%20bypass/pics%20for%20walktrough/2.png)

Введем код в нужное поле, и видим, что мы авторизовались, и заметим, что при авторизации, мы перешли на url = `https://0a21001104ea546f838b1f7c006b0060.web-security-academy.net/my-account`

Разлогинимся, и попытаемся авторизоваться под пользователем `carlos:montoya`.
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/9.%202FA%20simple%20bypass/pics%20for%20walktrough/4.png)

И когда нас перебросит на url `https://0a21001104ea546f838b1f7c006b0060.web-security-academy.net/login2` изменим `/login2` на `/my-account`, чтобы полный url получился 
`https://0a21001104ea546f838b1f7c006b0060.web-security-academy.net/my-account`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/9.%202FA%20simple%20bypass/pics%20for%20walktrough/5.png)
Отлично! Вот мы и выполнили лабораторную работу игнорируя необходимость ввода второго фактора
