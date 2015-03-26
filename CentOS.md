#Настройка CentOS

Обновляемся:

	sudo yum -y update

Устанавливаем Миднайт Командер:

	sudo yum install mc

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
    
Установка нужных пакетов

	yum install unzip zip sendmail htop
	
Установим MySQL:

	sudo yum install mysql-server mysql

Добавим его в автозапуск:

	sudo /sbin/chkconfig --levels 235 mysqld on

и запустим:

	sudo service mysqld start

После этого зададим пароль пользователю root:

	mysql -u root
	USE mysql

не забудьте вписать что-то свое вместо s10ZniYpar00L

	SET PASSWORD FOR 'root'@'localhost' = PASSWORD('s10ZniYpar00L');

Запретим подключение к базе без пароля:

	DELETE FROM user WHERE password = '';
	DELETE FROM user WHERE user.user= '';

и обновим привилегии:

	FLUSH PRIVILEGES;

Удалим тестовую базу:

	DROP DATABASE test;

и выйдем из консоли управления MySQL сервером:

	\q

Создаем группу sftp

	groupadd --system sftp
	
Добавляем в конец /etc/ssh/sshd_config

	Subsystem sftp internal-sftp
	Match Group sftp
	ChrootDirectory %h
	AllowTCPForwarding no
	ForceCommand internal-sftp

И комментируем там же

	#Subsystem sftp /usr/lib/openssh/sftp-server

перезапускаем sshd

	service sshd restart
