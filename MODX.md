Задаем права:

    sudo chmod 777 /var/www/site1.ru/core/cache/ /var/www/site1.ru/assets/ /var/www/site1.ru/core/packages/ /var/www/site1.ru/core/export core/config /var/www/site1.ru/core/components/
    sudo chmod -R 755 /var/www/site1.ru/
    sudo chown -R site1:site1 /var/www/site1.ru/
    sudo chown root:root /var/www/site1.ru
    
    sudo chmod 777 -R /var/lib/php/session
