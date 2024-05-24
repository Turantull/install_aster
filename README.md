# install_aster
1. Установить зависимости

sudo apt install -y build-essential autoconf libncurses5-dev \
libxml2-dev libsqlite3-dev curl mpg123 libxml2 \
libaudiofile-dev subversion sox libsox-fmt-all uuid-dev \
libjansson-dev libiksemel-dev libtiff5-dev 
 

sudo apt install -y bison flex libssl-dev \
libnewt-dev sqlite3 pkg-config automake libtool git unixodbc-dev \
uuid libasound2-dev libogg-dev libvorbis-dev libcurl4-openssl-dev libical-dev libneon27-dev \
libspandsp-dev libopus-dev opus-tools libiksemel-utils libiksemel3 xmlstarlet gcc make


2. Установка Asterisk

cd / &&
mkdir asterisk &&
cd asterisk/ &&
wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-16.15.1.tar.gz &&
http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-18.15.1.tar.gz
wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-18.9.0.tar.gz
wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-21.2.0.tar.gz
tar -zxvf asterisk-16* &&
cd asterisk-16.*/ &&
sudo contrib/scripts/install_prereq install -y &&
./configure --prefix=/asterisk/asterisk/ --with-jansson-bundled  &&

sudo make menuselect  &&
sudo make

sudo make install &&
sudo make samples &&
sudo make config

sudo make install-logrotate   #сказано в мануале Logrotate - это системная утилита, которая управляет автоматической ротацией и сжатием лог-файлов
_______________________________________________________________________________________________________________________
Дополнительно 

sudo ln -s /asterisk/asterisk/sbin/asterisk /sbin/asterisk   # Сделать сылку на астерискmc

crontab
0 0 * * * root /sbin/asterisk-cc -rx 'logger rotate'

---IPTABLES
sudo apt install iptables-persistent

sudo netfilter-persistent save
sudo netfilter-persistent reload

---FAIL2BAN
в logger.conf в строке messeges => вконце дописать security
в астере потом logger reload
в  /etc/fail2ban/jail.conf дописую в конце

Для шлюзов игнорайпи
ignoreip = 127.0.0.1/8 ::1 


[asterisk-callcentre]
enabled = true
filter = asterisk
action   = iptables-allports[name=ASTERISK, protocol=all]
logpath = /asterisk/asterisk-bb/var/log/asterisk/messages
maxretry=5

потом fail2ban-client reload
проверить состояние fail2ban-client status asterisk-callcentre


fail2ban-client set asterisk-callcentre unbanip ip   --РАЗБАНИТЬ

sudo fail2ban-client set ssh-iptables unbanip ip
