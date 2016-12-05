#Installation sur ubuntu 16.04

##Avant
- modifier hosts avec les bonnes ips
- apt-get update && apt-get upgrade
- installer vim et python: apt-get install -y python vim
- installer clef public pour utilisateur root
- Modifier le fichier /etc/ssh/sshd_config: port et PermitRootLogin


##créer une deuxième instance de redis

cp /etc/init.d/redis-server /etc/init.d/redis-server2

modifier:
DAEMON_ARGS=/etc/redis/redis-server2.conf
NAME=redis-server2
DESC=redis-server2
PIDFILE=$RUNDIR/redis-server2.pid

chmod a+x /etc/init.d/redis-server2

mv /etc/redis/redis.conf /etc/redis/redis-server2.conf

modifier:
pidfile /var/run/redis/redis-server2.pid
port 6380
logfile /var/log/redis/redis-server2.log
dir /var/lib/redis2


chown redis:redis /etc/redis/redis-server2.conf
mkdir /var/lib/redis2
chown redis:redis /var/lib/redis2
touch /var/log/redis/redis-server2.log
chown redis:redis /var/log/redis/redis-server2.log

/etc/init.d/redis-server2 start
update-rc.d redis-server2 defaults