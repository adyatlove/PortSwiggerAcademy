Залогинимся под `wiener:peter` и найдем запрос в `Proxy` -> `HTTP history` на аутентификацию и обязательно ставим галочку `stay logged in`!
```
POST /login HTTP
...
username=wiener&password=peter&stay-logged-in=on
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/1.png)

Видим, что в ответ нам выдало параметр `stay-logged-in=d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw`. Так называемая специальная кука для постоянной авторизации
Отправим значение параметра `stay-logged-in` в `Decoder` и выберем `Decode as base64`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/2.png)
Увидим, что результатом является выражение `wiener:51dc30ddc473d43a6011e9ebba6ca770`, где значение `51dc30ddc473d43a6011e9ebba6ca770` выглядит как хэш

Зайдем на любую крякалку хэшей, я в лабораторной выбрал 
```
https://crackstation.net/
```
И увидим, является ли данная последовательность символов хэшом, или нет
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/3.png)
Гипотеза оказалась верна, и мы нашли пароль, соответсвующий хэш-сумме

Итого получается, что уникальная кука, выдаваемая каждому пользователю есть результат данного выражения `base64(login:md5(password))`.

Перейдем на корневую страницу. Найдем данный запрос в `Proxy` -> `HTTP history` и отправим его в `Intruder`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/4.png)
В разделе `Positions` будем брутить по значению поля `stay-logged-in`
```
GET /
...
Cookie: stay-logged-in=§d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw§;
```
В разделе `Payloads` выберем `Simple list` и вставим предложенные автором лабораторной работы пароли
```
https://portswigger.net/web-security/authentication/auth-lab-passwords
```
Далее добавим в `Payload processing` следующие строки
* `Add -> Hash -> md5` - ибо нам нужно провести операцию по вычислению хэш-суммы нашего пароля
* `Add -> prefix : carlos:` - будем брутить УЗ карлоса, как это требует задание. Не забываем добавить `:`!
* `Add -> base64 encode` - всю комбинацию login:md5(password) нужно перекодировать в `base64`

После данной конфигурации запустим атаку
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/5.png)

Результат отсортируем по длинне возвращаемой страницы, и увидим, что одна из них отличвется и имеет параметр 
```
stay-logged-in=Y2FybG9zOjZkNGRiNWZmMGMxMTc4NjRhMDI4MjdiYWQzYzM2MWI5
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/6.png)

Отправим параметр `Y2FybG9zOjZkNGRiNWZmMGMxMTc4NjRhMDI4MjdiYWQzYzM2MWI5` в Decoder и выберем `Decode as base64`
и видим следующую комбинацию как ответ `carlos:6d4db5ff0c117864a02827bad3c361b9`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/7.png)
Отправим хэш сумму `6d4db5ff0c117864a02827bad3c361b9` в `https://crackstation.net/` и узнаем, есть ли у него в базе пароль, соответсвующей нашей хэш-сумме

И получаем пароль `moon`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/8.png)

Логинимся под `carlos:moon` и получаем уведомление об успешном выполнении лабораторной работы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/9.%20Brute-forcing%20a%20stay-logged-in%20cookie/pics%20for%20walktrough/9.png)
