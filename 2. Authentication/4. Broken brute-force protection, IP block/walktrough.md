Залогинимся под случайными кредами на сайте, и найдем в `Proxy` -> `HTTP history` данный запрос
```
POST /login
...
username=qwe&password=123
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/1.png)
Так же заметим, что сайт обладает защитой от брута логинов паролей, и после 3 неудачных попыток блокирует наш IP адрес, и выводит соответсвующую напись
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/2.png)
По условию задания, нам известны креды одного из существующий пользователей - wiener:carlos. Обратим внимание, что при заблокированном IP адресе когда мы вводим правильные и существующие креды - то блокировка обнуляется, и мы можем брутить дальше
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/3.png)
Отправим наш изначальный запрос в `Intruder` и выберем атаку `Pitchfork`, чтобы мы могли брутить по 2 параметрам сразу - по значению поля `username` и `password`
```
POST /login
...
username=§wiener§&password=§peter§
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/4.png)
В странице `Payloads` сконфигурируем лист логинов чередованием логинов `carlos (логин который мы брутим)` и логина `wiener (который мы используем чтобы обнулить счетчик неудачных логинов, чтобы не попасть в рейтлимиты)`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/5.png)
В качестве второго пэйлоада конфигурируем список из предлженного авторами лабы паролей 
```
https://portswigger.net/web-security/authentication/auth-lab-passwords
```
И на каждую вторую позицию этого списка вставляем пароль `peter`, чтобы связка `winer:peter` действительно сбрасывала счетчик неудачных аутентификаций
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/6.png)
При многочисленных запросах, отправляемых при бруте, сервер может обрабатывать полный поток не в том порядке, в котором мы ему послали. Даже при нашей чередовании может случится такая ситуация, что сервер обработает несколько попыток аутентификации под `carlos`, заблочит IP, и продолжит блочить дальнейший брут по `carlos`, а обработать логины `wiener` позже. Тем самым получаем, что наша идея по сбрасыванию блокировки не сработает

Чтобы избежать вышеописанной ситуации, нужно увеличить промежуток отправки наших запросов. В этом случае мы теряем в скорости времени проведения атаки, но выигрываем в том, что сервер обработает пэйлоады в правильном порядкее
Устанавливем кастомный пул, и ставим в него `Maximum concurrent requests : 1`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/7.png)
Запустим атаку
Отфильтруем результат атаки, и увидим, что единственный результат у `carlos` с `302 response status code` имеет пароль `computer`. Это и есть разыскиываемый пароль для УЗ `carlos`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/8.png)
Аутентифицируемся под `carlos:computer` и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/2.%20Authentication/4.%20Broken%20brute-force%20protection%2C%20IP%20block/pics%20for%20walktrough/9.png)


