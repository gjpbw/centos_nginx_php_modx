Установка NGINX
===================
 Добавляем  репозиторий, для этого, создаем файл с именем /etc/yum.repos.d/nginx.repo и таким содержимым:
 
    [nginx]
    name=nginx repo
    baseurl=http://nginx.org/packages/centos/6/$basearch/
	gpgcheck=0
    enabled=1
    priority=1


Устанавливаем:

	sudo yum install nginx

Добавляем nginx в автозагрузку:

	chkconfig --levels 235 nginx on

Запускаем:

	service nginx start

Должно появиться 

	Starting nginx:          [OK]

Основной конфигурационный файл Nginx (/etc/nginx/nginx.conf) приводим к такому виду (старый бэкапим):

    user                    nginx;
    worker_processes        2;
    error_log               /var/log/nginx/error.log;
    pid                     /var/run/nginx.pid;
    events {
                            worker_connections  1024;
    }
    http {
        include             /etc/nginx/mime.types;
        client_max_body_size 100m;
        access_log          /var/log/nginx/access.log;
        sendfile            on;
        keepalive_timeout   65;
        tcp_nodelay         on;
        gzip                on;
        gzip_min_length     1000;
        gzip_proxied        any;
        gzip_types          text/plain text/xml application/xml application/x-javascript text/javascript text/css text/json;
        gzip_disable        "msie6";
        gzip_comp_level     8;
        charset             utf-8;
        include             /etc/nginx/conf.d/*.conf;
        include             /etc/nginx/sites-enabled/*;
    }