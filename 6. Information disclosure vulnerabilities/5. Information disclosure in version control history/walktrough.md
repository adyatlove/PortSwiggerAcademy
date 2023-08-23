Изучим вектор уязвимости на незакрытые пути для систем контроля версий

Для этого перейдем по `/.git` на открывшемся ресурсе
```
https://0a3300c50496c3208130f85e003c00e7.web-security-academy.net/.git
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/5.%20Information%20disclosure%20in%20version%20control%20history/pics%20for%20walktrough/1.png)

Заметим, что страница действительно существует, и в ней лежат предыдущие комиты, в которых может располагаться сенсетив информация
Скопируем данный репозиторий к себе локально, чтобы открыть и посмотреть предыдущие версии
```
mkdir -p /home/kali/PortSwiggerLabs/lab5
cd /home/kali/PortSwiggerLabs/lab5
wget -r https://0a3300c50496c3208130f85e003c00e7.web-security-academy.net/.git
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/5.%20Information%20disclosure%20in%20version%20control%20history/pics%20for%20walktrough/2.png)

На следующем шаге откроем данную ветку любым средством контроля версий. Я открою через Git Kraken
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/5.%20Information%20disclosure%20in%20version%20control%20history/pics%20for%20walktrough/4.png)
И откроем файл `admin.conf`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/5.%20Information%20disclosure%20in%20version%20control%20history/pics%20for%20walktrough/5.png)
Отлично! мы получили ключ, под которым мы сможем залогиниться под УЗ administrator
`ADMIN_PASSWORD=xputq592dnuvkka1k81y`
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/6.%20Information%20disclosure%20vulnerabilities/5.%20Information%20disclosure%20in%20version%20control%20history/pics%20for%20walktrough/6.png)
После успешного логина удалим пользователя `carlos`, как это требует задание
