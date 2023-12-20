AngularJS — популярная библиотека JavaScript, которая сканирует содержимое узлов HTML, содержащих атрибут ng-app (также известный как директива AngularJS). Когда директива добавляется в HTML-код, вы можете выполнять выражения JavaScript в двойных фигурных скобках. Этот метод полезен при кодировании угловых скобок.


отправим рандомную квери для поиска и перейдем в прокси хттп хистори

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/8.%20DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/pics%20for%20walktrough/1.png)

увидим в ответе тэг

   `<body ng-app>`

`ng-app` есть триггер на ангуляр жс

на просторах хитгаба найдем пжйлоад
```
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md
```
```
{{constructor.constructor('alert(1)')()}}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/8.%20DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/pics%20for%20walktrough/2.png)

внедрим его и получим, что пэйлоад отработал, и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/8.%20DOM%20XSS%20in%20AngularJS%20expression%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded/pics%20for%20walktrough/3.png)
