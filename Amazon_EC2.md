#Настройка Amazon EC2

Обновляемся:

>   sudo yum -y update

Устанавливаем Миднайт Командер:

>   sudo yum install mc

Создадим пользовательскую группу www:

> sudo groupadd www

Добавляем пользователей (удобно, когда имя пользователя совпадаем с именем сайта. Например для сайта www.yandex.ru создаем пользователя yandex):

> sudo useradd site1 -g www -d /var/www/site1

Задаем пароли:

> sudo passwd site1


На всякий случай еще раз установим права на каталог:

> sudo chown site1:www /var/www/site1

> sudo chmod 0750 /var/www/site1


Повторяем все шаги для второго сайта (при необходимости для третьего, четвертого и т.д.)

> sudo useradd site2 -g www -d /var/www/site2

> sudo passwd site2

> sudo chown site2:www /var/www/site2

> sudo chmod 0750 /var/www/site2

Устанавливаем PHP:

>   sudo yum install php

