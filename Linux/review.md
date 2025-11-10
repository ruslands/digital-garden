Understanding PATH

When you run a command like python or pip, your operating system searches through a list of directories to find an executable file with that name. This list of directories lives in an environment variable called PATH, with each directory in the list separated by a colon:

/usr/local/bin:/usr/bin:/bin

Directories in PATH are searched from left to right, so a matching executable in a directory at the beginning of the list takes precedence over another one at the end. In this example, the /usr/local/bin directory will be searched first, then /usr/bin, then /bin.

systemctl --type=service --state=running

sudo passwd root

kill -s 9 2659

# create zip

zip files.zip file1 file2

# zip all files in directory

zip -r files.zip directory

# change permissions

chmod 400 keyfile.pem

# add to ssh agent

ssh-add keyfile.pem

# add to keychain

ssh-add --apple-use-keychain keyfile.pem

ssh -i file.pem username@ip-address

find / -type d -name 'gopherdata'

egrep -i 'ClientAliveInterva|ClientAliveCountMax' /etc/ssh/sshd_config

sudo vi /etc/ssh/sshd_config

ClientAliveInterval 30

ClientAliveCountMax 5

source or . :command can be used to read a file and treat its content as a set of commands to execute. the dot character is a synonym for the source keyword. source command creates variables in the same shell, while the bash command creates a new shell 

cal -3 :show calendar

Sniff https [https://habr.com/ru/company/dataart/blog/419677/](https://habr.com/ru/company/dataart/blog/419677/)

The commands adduser and useradd are used to create such Users. The main difference is that adduser sets up user folders, directories, and other necessary functions easily, whereas useradd creates a new user without adding the directories as mentioned above and settings.

also adduser is a wrapper for useradd

useradd -m valentin # create user without home directory

groupadd editorial # create group

usermod -a -G editorial valentin # add user to group

grep editorial /etc/group # check the list of users of certain group

etc/passwd # информация о пользователей  
etc/shadow  
etc/group # информация о группах

userdel user # удалит usera

chmod - modify file access rights  
su - temporarily become the superuser  
chown - change file ownership  
chgrp - change a file's group owner

chgrp -R editorial <dir> # change group ownership of everything inside the directory

drwxrwxr-x 2 alex consult 4096 Feb 20 10:10 some_dir/  
                            ^     ^  
                            |     |_____ directory's group  
                            |___________ directory's owner

chmod -R 700 app

chmod -R 770 vpakete-core

chmod -R 770 vpakete-yookassa

chmod -R 770 vpakete-warehouse

chmod 770 app

chgrp -R editorial app

chgrp -R editorial vpakete-core

chgrp -R editorial vpakete-yookassa

=============

/======System Info======/

Check CPU

lscpu

cpuid

Check Disks

ls -l /dev/ | grep sd #  check all disks

mount

df -h

lsblk

Memory Usage

free-m

cat /proc/meminfo

Hardware info

Lshw

lscpu

grep 'invalid' /var/log/auth.log

htop

iotop

du -sh -- * | sort -hr

# change timezone

sudo ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime

cat /etc/os-release

systemctl list-unit-files

du -h --max-depth=1 <folder_name> | sort -hr

ssh-keygen -t rsa -b 4096 -C "konovalov.rus@gmail.com"


ssh-add ~/.ssh/id_rsa_kodit

git clone git@git.seller-1.ru:rukonovalov:10022/rtk-config.git

Про cgi, python, php, apache нужно сразу забыть. Лучше забыть и про http и вебсокеты. А смотреть на C/C++, node.js, Go, in-memory базы данных, TCP сокеты.

printenv

printenv ACCESS_TOKEN

sudo apt-get install firefox  
wget [https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-linux64.tar.gz](https://github.com/mozilla/geckodriver/releases/download/v0.23.0/geckodriver-v0.23.0-linux64.tar.gz)  
tar -xvzf geckodriver*  
chmod +x geckodriver  
sudo mv geckodriver /usr/local/bin/# OR export PATH=$PATH:/usr/local/bin/geckodriver

!jupyter nbconvert --to script 'RSS.ipynb'

alias qwe="cd DATA/ && source JUPYTER/bin/activate && cd JUPYTER && jupyter notebook"  
sudo apt-get -f install # if dependence error  
sudo apt-get install sshfs # примонитровать сервер локально

# разрешить подключение к порту 5432 удаленно  
iptables -A INPUT  -m tcp -p tcp --dport 5432 -j ACCEPT  
export LC_ALL=C

pkill -u r.konovalov

shift+f2 -> rulers

/----Useful Programs----/  
Deluge # torrent client  
Playonlinux # Help install windows programs on Linux  
Gparted # manipulate disk  
Woeusb # загрузочгая флешка  
sudo apt-get install sqlitebrowser # смотреть таблицы БД  
sqlitebrowser # launch browser

sudo fuser -k 8000/tcp # убьет процесс на 8000 порту

  
DNS использует протоколы UDP, TCP на 53 порт  
MX - Mail eXchanger, указание серверов отвечающих за обработку почты в домене  
  
CNAME - canonical name, ссылка на другое доменное имя

sudo swapoff --all  
sudo swapon --all

gedir .bashrc # add new alias forever  
arch # разрядность системы  
lsb_release -a # версия ОС  
sudo lshw -c video # check videocard  
sudo apt-get autoclean # почистит кэш  
sudo apt-get autoremove # удалит зависимсти, котрые больше не нужны

etc/passwd # информация о пользователей  
etc/shadow  
etc/group # информация о группах

sudo su # станем root'ом  
ctrl + D # завершит сеcсию

userdel user # удалит usera

kill -s 9 id_process # убьет процесс по id сигнал 9 убить

Ntfs - для Windows  
Fat32 - для всех систем  
sudo umount /media/iso  
[http://webcodius.ru/sql/soedinenie-tablic-ili-dejstvie-operatora-sql-join-na-primerax.htmlML](http://webcodius.ru/sql/soedinenie-tablic-ili-dejstvie-operatora-sql-join-na-primerax.htmlML)  
swapoff -a && swapon -a &  
cron - планировщик задач linux  
linux command line - CTRL+R find previous commands  
     
/-------------------Linux---------------------/

Shell/bash - командный интерпретатор. Программа, которая предоставляет командный интерфейс взаимодействия с системой (командная строка).  
Authentication- проверка подлинности.  
Authorization- предоставление прав доступа. Соответствие каким-то критериям.  
Accounting - сохранение информации о деятельности пользователя.  
SSH - шифрует весь трафик  
SWAP - кусок оперативной памяти, который находится на диске.(Это очень медленно)

Сервис - процесс выполняющийся в фоне, отвязанный от терминала, имеющий ppid =  
nohup # отвяжет процесс от терминала, продолжит работать в фоне.

/dev/null # черная дыра  
/dev/zero # источник нулей  
/dev/random # источник случайных чисел  
/etc # локальные настройки, храянться настройки программ  
/home # каталоги, хранять домашние каталоги  
/bin # базовые утилиты, утилииты чтобы загрузить системы  
/sbin # сервисные утилиты, для настрйоки и обслуживания системы  
/usr # общая часть программ  
/var # изменяемые файлы(логи, очереди, базы, кэши)  
usr/local/$software # Если самосбор системы, то софт храним в этом пути

/bin contains the executables needed at boot time;

/usr/bin contains the other executables used by the OS installation;

/usr/local/bin contains executables installed by the system administrator that are not part of the base OS.

usr/src/soft        # тут должны храниться исходники программ  
syslog - логируются все действия системы  
var/log - ознакомиться с этой папкой  
etc/passwd # информация о пользователей  
etc/shadow  
etc/group # информация о группах  
cat /proc/cpuinfo/ # характеристики системы

./ # обратись к текущему каталогу, если требуется запустить программу из текущего каталога то следует добавить перед именем программы ./ (./programm)

/--------Управление пакетами-------------/

apt # управление пакетами  
apt-cache # инфо о пакете  
apt-get # управление пакетами

/--Install deb package--/  
sudo dpkg -i DEB_PACKAGE # если скачал пакет на компьютер, то ставим через dpkg  
apt-get install virtualenv # установка удаленно из репозитрия

/--Update--/  
apt-get update # обновит все удаленноые репозитории

apt-get search ^virtualenv # найдет все пакеты соответствующие запросу

/--Remove--/  
dpkg -r  
sudo apt-get remove your_app # удалить прорамму

dpkg -l # список установленных программ  
dpkg -l | grep python # grep поиск по регулярному выражению

virtualenv dpkg-query # покажет зависимости пакета

tar -xvzf /file.tar.gz # распаковка. x - распаковать, z - значит архив в zip  
./configure --prefix=/src/soft # укажем путь куда устанавливать пакет  
lzma -d /advert.log.lzma # распаковка файла advert.log.lzma

/-----Console Commands-----/  
ls dump2 | wc -l  
ctrl+z # выведет текущую программу в фон  
bg # продолжить ее выполнение в фоне  
fg # достать из фона  
jobs # фоновые задачи  
ctrl-d # завершает текущую сессию  
whoami #  
hostname #  
groups #  
id #  
uname -sro # информация о системе  
w # текущие сессиии  
who # текущие сессии  
hostname -I # check IP  
  
sudo su # станем root'ом  
ctrl + D # завершит сеcсию  
userdel user # удалит usera

free # покажет использование памяти и swap'a

kill -s 9 id_process # убьет процесс по id сигнал 9 убить

ls -la | grep str  
ls / # покажет корневые папки  
ls -la --sort=time # выведет файлы отсоритрованные по времени изменения.

find -name=filename -type=d(d-dir,f-file) # поиск файла/директорий  
locate # поиск файла/директорий  
which programm # покажет, где лежит исполняемый файл

ps aux # списое всех процессов  
htop # тоже самое но интерактивно

jobs # список фоновых процессов

tmux # сессия, которая поддерживает соединение постоянно  
CTRL+B+D - отключиться от сессии, но процесс останется

nmap # стучиться по всем портам сервера port knocking  
инфо безопасность  
записки касперского

man ls # показывает справку по всем командам  
ls # покажет всю информацию в этой директории  
ls -lah # покажет всю информацию в этой директории + скрытое  
mkdir -p a/b/c # создат три каталога  
tree a # покажет дерево каталога  
tree ~ -lah # покажет структуру корневой папки + скрытое

df -h # покажет место на дисках  
du -h file.txt # покажет размер файла  
df -i # покажет количество inode (индексный дескриптор, хранится информация о файле. 1 файл - 1 инода)

gedit - запуск текстового редактора  
find ~ -name ".ssh" - поиск  
touch file.txt - создать файл  
nano file.txt - редактировать файл  
lsb_release -a - версия системы  
sudo apt-get install something

split -l 200000 mybigfile.txt # разбить большой файл на несколько маленьких

sudo sysctl net.ipv4.ip_default_ttl=66 # сменить ttl на 2 для раздачи wi-fi

systemd-analyze - скорость загрузки системы

systemd-analyze blame - на что тратиться время при загрузке системы

Browser lynx # браузер командной строки

lscpu # покажет информацию о системе  
lspci # информация о подключенных системах

/-----Стандартный ввод/вывод-----/

echo "какая-то строка"  
cat file.txt # вывод содержимого файла  
cat file.txt | python program.py | … # | первый вертикальный разделитель значит - подать файл на стандартный вход  

python program.py > output.txt  
python program.py < input.txt  
python program.py < input.txt > output.txt  
   
Полезные команды:  
• less # просмотр файла  
• wc -l # подсчет числа строк в файле  
• sort # отсортировать то, что подается на вход  

head -n 10 file.txt # выведет первые 10 строчек  
tail -n 10 file.txt # выведет последние 10 строчек

cat file.txt | wc -l # считаем число строк в файле  
cat file.txt | sort | uniq -c  
cat file.txt | grep s # выводит только те строчки, которые содержат шаблон s

/-----Работа с терминалом-----/

Curl скачает, выведет на stdout файл не сохраняя  
wget скачать и сохранить в текущей папке

sudo chown -R rus:rus /home/rus/ # поменять права или сделать себя пользователем всех папок  
scp -i npl.pem result.json ruslan.konovalov@cluster.newprolab.com:~/data/home/ruslan.konovalov  
curl -r 0-1000 [http://data.newprolab.com/public-newprolab-com/advert.log](http://data.newprolab.com/public-newprolab-com/advert.log) # range of values от 0 до

/-----jupyter-----/

_7 # обратиться к 7 ячейке  
ctrl / # многострочный комментарий  
!pip list # shell command in jupyter  
%magic - руководство по magic командам

/---Загрузочная флешка---/  
1. Сделал загрузочную флешку с Windows8.1 на linux через woeusb  
2. Активировал windows через MKS  
3. Обновил Windows через ресурс для людей с ограниченными возможностями

gparted + unetbootin # создание загрузочной флешки linux distributiv  
gparted + woeusb # для windows (woeusb искать в поиске по компьютеру)

флешку форматируешь в FAT32 и копируешь на неё всё содержимое образа.  
Сделать раздел, сделать его активным, форматнуть в fat32, примонтировать, «распаковать» на этот раздел содержимое iso

  
alias qwe="cd DATA/ && source JUPYTER/bin/activate && cd JUPYTER && jupyter notebook"

1. Install zsh [https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)
2. Install zplug [https://github.com/zplug/zplug](https://github.com/zplug/zplug)

export ZPLUG_HOME=/usr/local/opt/zplug

source $ZPLUG_HOME/init.zsh

zplug "plugins/git", from:oh-my-zsh, depth:1

zplug "plugins/fzf", from:oh-my-zsh, depth:1

zplug "zsh-users/zsh-autosuggestions", from:github, depth:1

#zplug "marlonrichert/zsh-autocomplete", from:github, depth:1

zplug "zsh-users/zsh-syntax-highlighting", from:github, depth:1, defer:2

zplug "reegnz/jq-zsh-plugin",  from:github, depth:1

zplug "romkatv/powerlevel10k",  from:github, as:theme, depth:1

zplug "plugins/git"

zplug "plugins/fzf"

zplug "zsh-users/zsh-autosuggestions"

zplug "zsh-users/zsh-syntax-highlighting"

zplug "reegnz/jq-zsh-plugin"

zplug "romkatv/powerlevel10k"

#zplug "marlonrichert/zsh-autocomplete"

zplug load

zplug install

zplug is a function, not a program.

You need to find where brew put zplugs init.zsh and source it.

My ~/.zshrc contains the following:

if [ -f ${HOME}/.zplug/init.zsh ]; then  
    source ${HOME}/.zplug/init.zsh  
fi

# MacOS see usb devices

ioreg -p IOUSB

su username # действуем от имени другого пользователя  
ls/home # домашняя директория юзеров  
adduser, addgroup #

rwxrwxrwx # чтение-запись-исполнение  для владельца/для группы/для всех остальных

chown filename user # поменять собственника файла  
chgrp groupname filename # поменять группу владельца файла

tty - текущий терминал, при закрытии терминала все процессы выполняемы в нем  
закроются, nohup отвязывает процесс от терминала

telnet # установить tcp соединение telnet site.com

поставить nginx

env # переменная окружения, используется для передачи информации при запуске процесса  
Команды  
set VARNAME = value  
export VARNAME = value  
unset VARNAME = value

Каждый процесс имеет 3 файловых дескриптора  
stdin, stdout, stderr

man # manual  
vi, mcedit # текстовый редактор  
watch  
screen #

Ввести команду sudo passwd root

Ввести пароль

Подтвердить ввод пароля

chmod 400 macbook-old.pem

ssh -i macbook-old.pem ubuntu@[3.229.67.17](about://#ElasticIpDetails:PublicIp=3.229.67.17)

sudo passwd root

su root

scp -r root@3.229.67.17:/app/wolt /Users/konovalov/work

scp -r /Users/konovalov/work/wolt root@35.208.140.126:/app

scp -r root@103.125.217.63:/dump_2022-08-07.gz /Users/konovalov

scp /Users/konovalov/dump_2022-08-07.gz root@3.93.209.248:/app/allpapers-config/data

docker cp allpapers-solr:/opt/solr-8.7.0/server/solr/configsets/_default .

scp  -r clusters.txt root@185.87.194.174:/root/work

scp -P 23236 root@185.87.193.141:/db/ifx.zip .

scp -i /.ssh/id_rsa new.csv rkonovalov@78.140.220.145:/home/INSTAGRAM/notebooks

scp service-auth.zip root@107.20.136.150:/app

scp -r root@3.74.167.46:/kodit.zip /Users/konovalov

scp /Users/konovalov/work/1.zip root@3.229.67.17:/app/allpapers

/---Workstation---/

1. Run on remote machine

jupyter notebook --no-browser --port=8890 --allow-root

version: '3'

services:

  jupyter:

    image: jupyter/datascience-notebook:latest 

    container_name: jupyter

    user: root

    working_dir: /home/work

    environment:

      - GRANT_SUDO=yes

      - CHOWN_HOME=yes

      - NB_USER=work

    ports:

      - '8890:8888'

    volumes:

      - '/root/work:/home/work'

    networks:

      - default

networks:

  default:

    external:

      name: food-net

1. Run on local machine

ssh -N -f -L localhost:8890:localhost:8890 root@202.120.45.89 -p 2345

# Server Backup

rsync -avzpch -e 'ssh -p 23236' --progress root@185.87.193.141:/docker /Volumes/240_TOSHIBA/server.backup

rsync -avzpch -e 'ssh -p 23236' --progress root@185.87.193.141:/db /Volumes/240_TOSHIBA/server.backup

netstat -tulpn | grep :80 # check which process hold the port

service postgresql stop

du -sh * | sort -hr

clean var folder cache/log

Add to .bashrc

j="cd work/ && source JUPYTER/bin/activate && cd JUPYTER/data"

d="cd work/ && source DJANGO/bin/activate && cd DJANGO/data"

source ~/.bashrc # run this command

chmod +x filename.sh # make file executable

./filename.sh # execute file

cat /etc/passwd

find <folder_name> -type f | wc -l # count all files

adduser <user_name>

sudo adduser <username> sudo

passwd # change password

ssh-copy-id rus@95.183.12.214

/root/.ssh/authorized_keys

#Change timezone of server

date

sudo hwclock --show

timedatectl list-timezones | grep -i asia

timedatectl set-timezone 'Asia/Shanghai'

/---ssh port change---/

[https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html](https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html)

sudo vi /etc/ssh/sshd_config # ssh config file, edit file

sudo service ssh restart # or -> sudo systemctl restart sshd.service

sudo tail -500 /var/log/auth.log | grep 'sshd'

sudo netstat -vnaut # Active Internet connections (servers and established)

export LC_ALL=C

sudo nano /etc/systemd/system/<service_name> # сервисный файл сервиса


env

export VAR=1

unset VAR 

launchctl list | grep postgres

netstat -anp | grep 5432

du -sh -- * | sort -hr

=====lsof=====

lsof -t -i tcp:8000 | xargs kill -9

=====systemctl=====

The systemctl command manages both system and service configurations, enabling administrators to manage the OS and control the status of services.

systemctl list-unit-files --state=enabled

sudo systemctl status <service_name>

sudo systemctl stop <service_name>

sudo systemctl start <service_name>

sudo systemctl restart <service_name>

sudo systemctl reload <service_name>

sudo systemctl disable <service_name>

sudo systemctl enable <service_name>

sudo service <service_name> status

=====brew=====

[https://brew.sh](https://brew.sh)

brew list                                            # list of tools

brew services list                            # list of active services

brew leaves                                      # shows you all top-level packages; packages that are not dependencies

brew services start postgresql

brew services start redis

brew tap homebrew/services

==> Tools

ant                    

fribidi                

graphite2              

krb5                  

libtiff                

libxrender             

openexr                

python@3.9

aom                    

fzf                    

graphviz               

libavif                

libtool                

lightgbm              

openjdk                

readline

autoconf               

gd                     

gts                    

libev                  

libunistring           

lz4                    

openssl@1.1            

sqlite

brotli                

gdbm                   

harfbuzz               

libffi                 

libuv                  

lzo                    

pango                 

telnet

c-ares                 

gdk-pixbuf             

icu4c                  

libidn2                

libvmaf                

m4                     

pcre                   

webp

ca-certificates        

gettext                

imath                  

libnghttp2             

libx11                 

maven                  

pixman                 

wget

cairo                  

giflib                 

jasper                 

libomp                 

libxau                 

mpdecimal              

pkg-config             

xorgproto

cocoapods              

glib                   

jemalloc               

libpng                 

libxcb                 

netpbm                 

postgresql             

xz

fontconfig             

gobject-introspection  

jpeg                   

libpthread-stubs       

libxdmcp               

nghttp2               

pyenv                  

yarn

freetype               

gradle                

jpeg-xl                

librsvg                

libxext                

node                   

python@3.10            

zplug

==> Casks

adoptopenjdk8          

android-ndk            

android-platform-tools  android-sdk            

android-studio         

keepassxc              

keypad-layout          

temurin