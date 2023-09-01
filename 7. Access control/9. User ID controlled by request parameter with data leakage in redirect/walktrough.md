Залогинимся под данным пользователем `wiener:peter`
```
https://0abc00870411c533805af80a001d00dd.web-security-academy.net/my-account?id=wiener
```
И найдем данный запрос в `Proxy -> HTTP history` и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/9.%20User%20ID%20controlled%20by%20request%20parameter%20with%20data%20leakage%20in%20redirect/pics%20for%20walktrough/1.png)

Далее изменим параметр `id=wiener`, на `id=carlos`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/7.%20Access%20control/9.%20User%20ID%20controlled%20by%20request%20parameter%20with%20data%20leakage%20in%20redirect/pics%20for%20walktrough/2.png)

По полученной html странице получим `API key` для `carlos`, и сдадим его в качестве ответа на задание 

`Your API Key is: 2JpsL0mIm8VSTxWsgeJakel2tAvlTDrZ`
