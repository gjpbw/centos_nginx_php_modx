Сразу после установки:	
	
	yum update  
	yum -y install mc  


Установка MariaDB (используется вместо MySQL)

	yum install -y mariadb-server mariadb
	systemctl enable mariadb
	systemctl start mariadb

Скрипт , повышения безопасности:

	mysql_secure_installation
	
Система спросит текущий root-пароль. Но поскольку система MySQL только что установлена, такого пароля пока что нет, потому просто нажмите enter. Затем вас спросят, хотите ли вы установит пароль, введите Y и следуйте инструкциям.
На все остальные вопросы просто нажмите enter.



Если нужно подключить сетевые диски nfs:

	yum install nfs-utils 
	systemctl enable rpcbind
	systemctl start rpcbind

Примеры записи в файле /etc/fstab

	192.168.9.15:/mnt/RAID/qqq.ru       /var/www/qqq/www/ext    nfs     noexec,rw,auto  0 0  
	192.168.9.5:/mnt/Data/system        /var/www/www.system/www nfs     noexec,ro,auto  0 0  
