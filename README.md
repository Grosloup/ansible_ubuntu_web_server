#Installation sur ubuntu 16.04

##Avant
- modifier hosts avec les bonnes ips
- mot de passe mysql (mysql)
- modifier ports ufw (security => ssh,redis,postfix)
- apt-get update && apt-get upgrade
- installer vim et python: apt-get install -y python vim
- installer clef public pour utilisateur root
- Modifier le fichier /etc/ssh/sshd_config: port et PermitRootLogin

##letsencrypt
www.ssllabs.com
le server à certifier est déjà configuré et écoute sur le port 80.
Dans les configs de celui-ci il faut que soit présente la règle suivante:
location ~ /\.well-known { allow all; } 
Attention le dossier .well-known doit être à la racine du document root

$ letsencrypt certonly --rsa-key-size 4096 --webroot -w <webroot> -d <hostname> -d www.<hostname> --email tech@zoobeauval.com

les configs du server peuvent être modifiées pour passer en https

###renouvellement automatique
lancer une première fois
$ letsencrypt renew 

modifier le crontab
$ crontab -e
30 3 * * 0 letsencrypt renew >> /var/log/letsencrypt.log >/dev/null 2>&1

##Ne pas oublier

- config de fail2ban

```
/etc/fail2ban

cp jail.conf jail.local


vim jail.local.conf

-> destemail

-> mta = mail   # postfix

-> action  = %(action_mwl)s

jails :

ssh changer le port

```




- logrotate

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