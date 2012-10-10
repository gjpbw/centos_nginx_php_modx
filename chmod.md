Скрипт для назначения прав для директорий\файлов. Рекомендую хранить в /var/www/site1/chmod

    #!/bin/bash
    user=site1
    dir=/var/www/$user/www
    
    chown -R $user:$user "$dir";
    find "$dir" -type d -exec chmod 0755 '{}' \;
    find "$dir" -type f -exec chmod 0644 '{}' \;
    
 Что бы сделать его исполняемым надо:  
 	
    chmod +x chmod
    
 