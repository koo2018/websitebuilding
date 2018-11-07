#!/bin/bash

#
# Здесь можно менять параметры (пробелы вокруг знака равно будут ошибкой)
#

# пароль администратора базы данных
rootdbpasswd=""

# Дальше править не нужно, если вы не понимаете, что делаете

apache_vhosts="/etc/apache2/sites-available"

if [ "$rootdbpasswd" = "" ]; then 
echo -e "Не указан пароль администратора базы данных \$rootdbpasswd. \n"
stop_var=1
fi


if [ "$#" -eq 0 ]; then
echo -e 'Для работы программы требуется один параметр\n
Формат команды \n
./delstudent studentName \n'
stop_var=1
fi




grep "$1:" /etc/passwd >/dev/null
if [ $? -ne 0 ]; then
echo -e 'Пользователя '$1' не существует\n'
stop_var=1
fi

if [ $stop_var ]; then 
echo -e "Скрипт остановлен \n"
exit 
fi

clear

echo "* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *"
echo "*"
echo "*  Удаляем студента " $1
echo "*"
echo "* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *"
echo ""

echo -n "Удаление пользователя "$1"...  "
sudo deluser  --remove-home $1
echo "Завершено"
echo ""

echo -n "Выключение сайта "$1" на сервере apache2...  "
sudo a2dissite -q $1
echo ""

echo -n "Удаление конфигурации "$1".conf на сервере apache2...  "
sudo rm -f $apache_vhosts"/"$1.conf
echo "Завершено"
echo ""

echo -n "Перезапуск apache2...  "
sudo systemctl reload apache2
echo "Завершено"
echo ""

echo -n "Удаление пользователя MySQL и базы данных...  "
mysql -u root -p$rootdbpasswd <<EOF
DROP USER $1@'localhost';
DROP DATABASE $1;
FLUSH PRIVILEGES;
EOF
echo "Завершено"
echo ""

echo "Удаление аккаунта пользователя "$1" завершено!"
echo ""
