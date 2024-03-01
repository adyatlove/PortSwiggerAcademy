Делаем рандомный запрос на данный ресурс, обновляем страницу и обращаем внимание в какие поля произошла запись нашего комментария

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/21.%20Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/pics%20for%20walkthrough/1.png)
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/21.%20Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/pics%20for%20walkthrough/2.png)
```
<img src="/resources/images/avatarDefault.svg" class="avatar">
<a id="author" href="http://qq.ru" onclick="var tracker={track(){}};tracker.track('http://qq.ru');">1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/21.%20Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/pics%20for%20walkthrough/3.png)

Когда контекстом XSS является какой-либо существующий JavaScript в атрибуте тега в кавычках, например обработчик событий, можно использовать HTML-кодирование для обхода некоторых входных фильтров.

Когда браузер проанализировал HTML-теги и атрибуты в ответе, он выполнит HTML-декодирование значений атрибутов тега перед их дальнейшей обработкой. Если серверное приложение блокирует или очищает определенные символы, необходимые для успешного использования XSS-эксплойта, вы часто можете обойти проверку ввода, закодировав эти символы в HTML.

Например, если контекст XSS выглядит следующим образом:
```
<a href="#" onclick="... var input='здесь управляемые данные'; ...">
```
и приложение блокирует или экранирует символы одинарных кавычек, вы можете использовать следующую полезную нагрузку, чтобы вырваться из строки JavaScript и выполнить свой собственный скрипт:
```
&apos;-alert(document.domain)-&apos;
```
`&apos;` последовательность — это объект HTML, представляющий апостроф или одинарную кавычку. Поскольку браузер HTML-декодирует значение атрибута onclick до интерпретации JavaScript, объекты декодируются как кавычки, которые становятся разделителями строк, и поэтому атака оказывается успешной.

Далее нам необходимо выйти за пределы ковычек `oneclick`

Для этого вставим символ заменяющий ковычки в поле ввода url сайта
и получим 
```
http://&apos;-alert(1)-&apos;
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/21.%20Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/pics%20for%20walkthrough/4.png)
из-за того, что внутренний ресурс экранирует `&apos;` как `'`, то мы вывалились за строку и у нас отработал XSS
и получаем уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/21.%20Stored%20XSS%20into%20onclick%20event%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20and%20backslash%20escaped/pics%20for%20walkthrough/5.png)
