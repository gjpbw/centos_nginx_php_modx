Установка NGINX
===================
 Добавляем  репозиторий, для этого, создаем файл с именем /etc/yum.repos.d/nginx.repo и таким содержимым:
>    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/6/$basearch/
    gpgcheck=0
    enabled=1
    priority=1
>

Устанавливаем:
> sudo yum install nginx

Добавляем nginx в автозагрузку:
> chkconfig --levels 235 nginx on

Запускаем:
> service nginx start

Должно появиться 
> Starting nginx:          [OK]

Создаем нужные каталоги:
>	mkdir /var/www

>	mkdir /var/www/site1

>	mkdir /var/www/site1/www

>	mkdir /var/www/site1/tmp

