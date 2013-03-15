#Настройка Amazon EC2

Обновляемся:
	sudo yum -y update

Устанавливаем Миднайт Командер:
	sudo yum install mc

Создадим пользовательскую группу www:
	sudo groupadd site1
    
Добавляем пользователей (удобно, когда имя пользователя совпадаем с именем сайта. Например для сайта www.yandex.ru создаем пользователя yandex):
	sudo useradd site1 -g site1 -d /var/www/site1
	
Создаем нужные каталоги:
	mkdir /var/www/site1/www
	mkdir /var/www/site1/tmp

Задаем пароли:
	sudo passwd site1

Повторяем все шаги для второго сайта (при необходимости для третьего, четвертого и т.д.)

	sudo useradd site2 -g site2 -d /var/www/site2
	mkdir /var/www/site2/www
	mkdir /var/www/site2/tmp
	sudo passwd site2
    
Устанавливаем PHP:
	sudo yum install php
    
Отдельной строчкой регэкспы:

	sudo yum install pcre-devel

и общей кучей модули и зависимости для PHP:

	sudo yum -y install aspell aspell-en aspell-ru cvs php-gd php-intl php-mbstring php-mysql php-pdo php-soap php-xml php-xmlrpc php-pspell php-devel php-pear
    
Настраиваем (редактируем /etc/php.ini):
    post_max_size = 100M
    
    upload_max_filesize = 100M
    
    disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_multi_exec,parse_ini_file,show_source
    
    cgi.fix_pathinfo = 0
    
    open_basedir = /var/www/
    
    date.timezone = Europe/Moscow
