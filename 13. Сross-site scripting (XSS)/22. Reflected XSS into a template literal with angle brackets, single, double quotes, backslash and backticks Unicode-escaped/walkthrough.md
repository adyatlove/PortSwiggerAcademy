зайдем на уязвимый ресурс и введем в поиске инфу о случайной литерало-циферной информации `abc123`
и увидим, что у нас появилась информация в поле вывода сообщения, что ничего не нашлось
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/22.%20Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets%2C%20single%2C%20double%20quotes%2C%20backslash%20and%20backticks%20Unicode-escaped/pics%20for%20walkthrough/1.png)

Шаблонные литералы JavaScript - это строковые литералы, которые позволяют встраивать выражения JavaScript. Встроенные выражения вычисляются и обычно объединяются в окружающий текст. Шаблонные литералы инкапсулируются в обратные кавычки вместо обычных кавычек, а встроенные выражения идентифицируются с помощью синтаксиса $ { ... }.

Например, следующий сценарий напечатает приветственное сообщение, содержащее отображаемое имя пользователя:
```
document.getElementById('message').innerText = `Welcome, ${user.displayName}.`;
```

Когда контекст XSS находится в литерале шаблона JavaScript, нет необходимости завершать литерал. Вместо этого вам просто нужно использовать синтаксис ${...} для встраивания выражения JavaScript, которое будет выполняться при обработке литерала. Например, если контекст XSS выглядит следующим образом:
```
<script>
...
var input = `controllable data here`;
...
</script>
```

затем вы можете использовать следующую полезную нагрузку для выполнения JavaScript без завершения литерала шаблона:

`${alert(document.domain)}`

Соответсвенно, изменим запрос на 
```
${alert(1)}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/22.%20Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets%2C%20single%2C%20double%20quotes%2C%20backslash%20and%20backticks%20Unicode-escaped/pics%20for%20walkthrough/2.png)

Отправим запрос и увидим собщение об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/22.%20Reflected%20XSS%20into%20a%20template%20literal%20with%20angle%20brackets%2C%20single%2C%20double%20quotes%2C%20backslash%20and%20backticks%20Unicode-escaped/pics%20for%20walkthrough/3.png)
