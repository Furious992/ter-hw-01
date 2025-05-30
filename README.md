Задание 1  
1. Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.  
2. Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)  
3. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.  
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.  
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.  
6. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.  
7. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.  
8. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

Решение 1.

![img](https://github.com/Furious992/ter-hw-01/blob/main/2.jpg)  
необходимо сделать инициализацию  
![img](https://github.com/Furious992/ter-hw-01/blob/main/3.jpg)  
В.gitignore в файле personal.auto.tfvars можно хранить секретные данные  
![img](https://github.com/Furious992/ter-hw-01/blob/main/4.jpg)  
После выполнения стейта получаем секретное значение random_password  
![img](https://github.com/Furious992/ter-hw-01/blob/main/5.jpg)  
В terrafrom validate ошибки, не указан тип ресурса для docker_image, цифра в названии, название задано random_string, random_string_FAKE и еще RESULT с большой буквы.  
![img](https://github.com/Furious992/ter-hw-01/blob/main/6.jpg)  

Исправляю и проверяю ещё раз  

```
resource "docker_image" "nginx" { name = "nginx:latest" keep_locally = true }
resource "docker_container" "nginx" { image = docker_image.nginx.image_id name = "example_${random_password.random_string.result}"
ports { internal = 80 external = 9090 } }
```

![img](https://github.com/Furious992/ter-hw-01/blob/main/7.jpg)  

Terraform plan  
![img](https://github.com/Furious992/ter-hw-01/blob/main/8.jpg)  

Изменил имя контейнера на hello_maxim  
![img](https://github.com/Furious992/ter-hw-01/blob/main/9.jpg)  
Запустил terraform apply -auto-approve.  
![img](https://github.com/Furious992/ter-hw-01/blob/main/10.jpg) 

Делаю destroy  
![img](https://github.com/Furious992/ter-hw-01/blob/main/11.jpg)  

Образ nginx:lates не был удален, потому что использовали параметр keep_locally = true 

[документация](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image)
