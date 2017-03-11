Качаем нужную версию для сервера.

    wget -O dropbox.tar.gz "http://www.dropbox.com/download/?plat=lnx.x86"  
    или  
    wget -O dropbox.tar.gz "http://www.dropbox.com/download/?plat=lnx.x86_64"

Если не знаете, что у вас за ОС, выполните команду

    uname -a

У меня вот такой вывод:

    Linux dev 2.6.32-642.15.1.el6.x86_64 #1 SMP Fri Feb 24 14:31:22 UTC 2017 x86_64

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

	# chkconfig: 345 85 15
	# description: Startup script for dropbox daemon
	#
	# processname: dropboxd
	# pidfile: /var/run/dropbox.pid
	# config: /etc/sysconfig/dropbox
	#
	 
	### BEGIN INIT INFO
	# Provides: dropboxd
	# Required-Start: $local_fs $network $syslog
	# Required-Stop: $local_fs $syslog
	# Should-Start: $syslog
	# Should-Stop: $network $syslog
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: Start up the Dropbox file syncing daemon
	# Description:       Dropbox is a filesyncing sevice provided by dropbox.com
	#                    This service starts up the dropbox daemon.
	### END INIT INFO
	 
	# Source function library.
	. /etc/rc.d/init.d/functions
	 
	# To configure, add line with DROPBOX_USERS="user1 user2" to /etc/sysconfig/dropbox
	# Probably should use a dropbox group in /etc/groups instead.
	 
	[ -f /etc/sysconfig/dropbox ] && . /etc/sysconfig/dropbox
	prog=dropboxd
	lockfile=${LOCKFILE-/var/lock/subsys/$prog}
	config=${CONFIG-/etc/sysconfig/dropbox}
	RETVAL=0
	 
	start() {
	    echo -n $"Starting $prog"
	    if [ -z $DROPBOX_USERS ] ; then
	        echo -n ": unconfigured: $config"
	        echo_failure
	        echo
	        rm -f ${lockfile} ${pidfile}
	        RETURN=6
	        return $RETVAL
	    fi
	    for dbuser in $DROPBOX_USERS; do
	        daemon --user $dbuser /bin/sh -c "/home/$dbuser/.dropbox-dist/dropboxd&"
	    done
	    RETVAL=$?
	    echo
	    [ $RETVAL = 0 ] && touch ${lockfile}
	    return $RETVAL
	}
	 
	status() {
	    for dbuser in $DROPBOX_USERS; do
	        dbpid=`pgrep -u $dbuser dropbox | grep -v grep`
	        if [ -z $dbpid ] ; then
	            echo "dropboxd for USER $dbuser: not running."
	        else
	            echo "dropboxd for USER $dbuser: running (pid $dbpid)"
	        fi
	    done
	}
	 
	stop() {
	    echo -n $"Stopping $prog"
	    for dbuser in $DROPBOX_USERS; do
	        killproc /home/$dbuser/.dropbox-dist/dropbox
	    done
	    RETVAL=$?
	    echo
	    [ $RETVAL = 0 ] && rm -f ${lockfile} ${pidfile}
	}
	 
	# See how we were called.
	case "$1" in
	    start)
	        start
	        ;;
	    status)
	        status
	        ;;
	    stop)
	        stop
	        ;;
	    restart)
	        stop
	        start
	        ;;
	    *)
	        echo $"Usage: $prog {start|status|stop|restart}"
	        RETVAL=3
	esac
	exit $RETVAL

Создаем файл /etc/sysconfig/dropbox с содержимым:

	DROPBOX_USERS="linux_user_name"
	linux_user_name=имя пользователя linux под кем запускается dropbox

Изменяем права на файл:

	chmod +x /etc/init.d/dropbox
	chkconfig --add dropbox
	chmod 755 /etc/init.d/dropbox

Добавляем в автозапуск	
	
	chkconfig dropbox on
