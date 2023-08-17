Войдем под пользователем `wiener:peter`

После загрузки корневой страницы попробуем просканировать данный ресурс на имеюющиеся в нем директории Для этого перейдем в `Target` -> `Site map` -> `Engagement tools` -> `Discover content`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/2.png)

Видим, что мы насканили директорию для админов

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/3.png)
но доступа у нас к ней нет
`https://0a0f003a036dcecb8149da4a00280057.web-security-academy.net/admin`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/4.png)
Выйдем из аккаунта. 
Далее зайдем в режим `Proxy` и включим `Intercept on`
и снова зайдем под аккаунтом `wiener:peter`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/6.png)
Далее необходимо `Drop` следующий `GET` запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/7.png)
Как результат, мы получим ошибку. Далее необходомо изменить url с 
`https://0a0f003a036dcecb8149da4a00280057.web-security-academy.net/role-selector`
на 
`https://0a0f003a036dcecb8149da4a00280057.web-security-academy.net/`
То есть просто перейти на главную страницу
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/8.png)
И мы заметим, что если мы НЕ выбирали с вами роль, положенную данной учетной записи, то ей по умолчанию присваиваются права Администратора, с доступом в `Admin panel` соответсвенно
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/9.png)

Зайдем в нее, у удалим `carlos`, как нам это и предписывает задание
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/5.%20Business%20logic%20vulnerabilities/11.%20Authentication%20bypass%20via%20flawed%20state%20machine/pics%20for%20walktrough/10.png)
