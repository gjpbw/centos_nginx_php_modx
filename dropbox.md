Качаем нужную версию для сервера.

    wget -O dropbox.tar.gz "http://www.dropbox.com/download/?plat=lnx.x86"  
    или  
    wget -O dropbox.tar.gz "http://www.dropbox.com/download/?plat=lnx.x86_64"

Если не знаете, что у вас за ОС, выполните команду

    uname -a

У меня вот такой вывод:

    Linux bezumkin 3.1.0-1.2-xen #1 SMP Wed Dec 7 19:01:22 MSK 2011 i686 GNU/Linux

i686 — это 32 битная ОС.

Распаковываем

    tar -xzvf dropbox.tar.gz

Запускаем

	~/.dropbox-dist/dropboxd

В первый раз он выведет ссылку на сервис, которую вам надо открыть у себя на компе и добавить этот сервер в число компьютеров аккаунта.

Пока не добавите — он так и будет выдавать эту ссылку. После добавления перезапускаем сервис (ctrl+c для прекращения работы) и мы должны увидеть "Client successfully linked, Welcome!"

//вышеуказанное содрано отсюда http://bezumkin.ru/sections/hosting/261/
//т.к. по ссылки выше для Ubuntu, а  у нас CentOS то остальное отличается:

Создаем файл /etc/init.d/dropbox с содержимым:
