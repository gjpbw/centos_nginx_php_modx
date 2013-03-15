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
    
Стандартный конфиг сайта /etc/nginx/sites-available/site1.conf

    upstream backend-site1 {server unix:/var/run/php5-site1.ru.sock;}
    server {
        listen              80;
        server_name         site1.ru;
        root                /var/www/site1.ru/www;
        access_log          /var/log/nginx/site1-access.log;
        error_log           /var/log/nginx/site1-error.log;
        index               index.php;
        rewrite_log         on;
        location /core/ {                                                                                                                                                                                                                               
            deny all;                                                                                                                                                                                                                                                    
        }
        location / {
            try_files       $uri $uri/ @rewrite;
        }
        location @rewrite {
            rewrite         ^/(.*)$ /index.php?q=$1;
        }
        location ~ \.php$ {
            include         fastcgi_params;
            fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_pass    backend-site1;
        }
        location ~* ^.+\.(jpg|jpeg|gif|css|png|js|ico|bmp)$ {
           access_log       off;
           expires          10d;
           break;
        }
        location ~ /\.ht {
            deny            all;
        }
    }
    
    
Устанавливаем: 
	sudo yum install php-fpm

создаем конфиг /etc/php-fpm.d/site1.conf:

    [site1]
    listen = /var/run/php5-site1.ru.sock
    listen.mode = 0666
    user = site1
    group = site1
    chdir = /var/www/site1.ru
    php_admin_value[upload_tmp_dir] = /var/www/site1.ru/tmp
    php_admin_value[soap.wsdl_cache_dir] = /var/www/site1.ru/tmp
    php_admin_value[date.timezone] = Europe/Moscow
    # тут значения можно поменять, в зависимости от нагрузки на сайт
    pm = dynamic
    pm.max_children = 10
    pm.start_servers = 2
    pm.min_spare_servers = 2
    pm.max_spare_servers = 4
    
Перезапускаем сервисы:

    service nginx restart
    service php-fpm restart
