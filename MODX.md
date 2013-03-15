Задаем права:

    sudo chmod 777 /var/www/site1/core/cache/ /var/www/site1/assets/ /var/www/site1/core/packages/ /var/www/site1/core/export core/config /var/www/site1/core/components/
    sudo chmod -R 755 /var/www/site1/
    sudo chown -R site1:site1 /var/www/site1/
    sudo chown root:root /var/www/site1
    
    sudo chmod 777 -R /var/lib/php/session
