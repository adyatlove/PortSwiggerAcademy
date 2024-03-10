```
https://YOUR-LAB-ID.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
```
Эксплойт использует обработку исключений для вызова функции оповещения с аргументами. Используется оператор throw, разделенный пустым комментарием, чтобы обойти ограничение отсутствия пробелов. Функция оповещения назначается обработчику исключений onerror.

Поскольку throw - это оператор, он не может быть использован в качестве выражения. Вместо этого нам нужно использовать функции стрелок для создания блока, чтобы можно было использовать оператор throw. Затем нам нужно вызвать эту функцию, поэтому мы присваиваем ее свойству toString window и запускаем его, принудительно Преобразуя строку в window.
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/28.%20Reflected%20XSS%20in%20a%20JavaScript%20URL%20with%20some%20characters%20blocked/pics%20for%20walkthrough/1.png)
