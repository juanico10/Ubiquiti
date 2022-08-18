# Wiki Ubiquiti
A collection of enhancements for EdgeMax based devices

## Configuración inicial del EdgeRouter 4

### Comandos básicos
commit: para activar los cambios.
save: para almacenar la configuración "activa" en la configuración de inicio. 
compare: Para ver qué cambios se han realizado en la configuración.
configure: modo configuración.
show: mostrar
set: establecer configuración.
edit: Cambiat el nivel de edición.
up:
top:
discard: para deshacer los cambios no confirmados
copy:
rename:
load: cargar configuración.

### Configurar usuario

#### Inicie sesión en el router y añada un nuevo usuario

```sh
configure
set system login user <user> authentication plaintext-password <secret>
set system login user <user> level admin
commit; save
```

#### Remover default user

```sh
configure
delete system login user ubnt
commit; save
```

#### Añadir una clave ssh pública a EdgeRouter

```sh
$ scp ~/.ssh/id_rsa.pub <ip-of-edgerouter>:/tmp
```

```sh
configure  
loadkey <user> /tmp/id_rsa.pub  
sudo chown -R <user> /home/<user>
commit; save
```

#### Comprobación de acceso

```sh
$ ssh <user>@<ip-of-edgerouter>
exit
```

#### Desactivar la autenticación de contraseñas en texto plano
Si puede iniciar sesión con éxito en el EdgeRouter, un paso para reforzar la seguridad de su EdgeRouter es eliminar la opción de utilizar una contraseña de texto simple.  
**NOTE!** Asegúrate de que puedes acceder con tu clave pública antes de desactivar la autenticación en texto plano.

```sh
configure
set service ssh disable-password-authentication
commit; save
```
#### Aseguar acceso a la GUI y ssh
```sh
configure
set service gui listen-address <lan ip address/range>
set service ssh listen-address <lan ip address/range>
set service gui older-ciphers disable
set service ubnt-discover disable
set service ssh protocol-version v2
delete service telnet
commit; save
```


### Setup Edgerouter 4

#### Firewall

```sh
configure
set firewall all-ping enable
set firewall broadcast-ping disable
set firewall ipv6-receive-redirects disable
set firewall ipv6-src-route disable
set firewall ip-src-route disable
set firewall log-martians enable
set firewall receive-redirects disable
set firewall send-redirects enable
set firewall source-validation disable
set firewall syn-cookies enable
set firewall name WAN_IN default-action drop
set firewall name WAN_IN enable-default-log
set firewall name WAN_IN rule 1 action accept
set firewall name WAN_IN rule 1 description "Allow established connections"
set firewall name WAN_IN rule 1 state established enable
set firewall name WAN_IN rule 1 state related enable
set firewall name WAN_IN rule 2 action drop
set firewall name WAN_IN rule 2 log enable
set firewall name WAN_IN rule 2 description "Drop invalid state"
set firewall name WAN_IN rule 2 state invalid enable
set firewall name WAN_LOCAL default-action drop
set firewall name WAN_LOCAL enable-default-log
set firewall name WAN_LOCAL rule 1 action accept
set firewall name WAN_LOCAL rule 1 description "Allow established connections"
set firewall name WAN_LOCAL rule 1 state established enable
set firewall name WAN_LOCAL rule 1 state related enable
set firewall name WAN_LOCAL rule 2 action drop
set firewall name WAN_LOCAL rule 2 log enable
set firewall name WAN_LOCAL rule 2 description "Drop invalid state"
set firewall name WAN_LOCAL rule 2 state invalid enable
commit; save
```

#### WAN

```sh
configure
set interfaces ethernet eth0 description WAN
set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth0 firewall in name WAN_IN
set interfaces ethernet eth0 firewall local name WAN_LOCAL
set service nat rule 5010 description "Masquerade for WAN"
set service nat rule 5010 outbound-interface eth0
set service nat rule 5010 type masquerade
commit; save
```

#### LAN
**NOTE!** Asegúrate de cambiar el rando de la red a la de tu red.
```sh
configure
set interfaces ethernet eth3 description LAN
set interfaces ethernet eth3 address 192.168.1.1/24
set service dhcp-server disabled false
set service dhcp-server shared-network-name LAN authoritative enable
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 default-router 192.168.1.1
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 dns-server 192.168.1.1
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 lease 86400
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 start 192.168.1.150 stop 192.168.1.254
set service dns forwarding listen-on eth3
commit; save
```

## POSIBILIDAD DE AÑADIR UN CERTIFICADO A LOCALHOST
**NOTE!** 

#### Añadir certificado CA para localhost
Usando [mkcert](https://words.filippo.io/mkcert-valid-https-certificates-for-localhost/) por [Filippo Valsorda](https://filippo.io/) para crear un certificado CA para localhost.  
Opción de ir a la ruta de certificados SSL con Let's Encrypt hay varios diferentes para elegir. Por ejemplo, [ubnt-letsencrypt](https://github.com/j-c-m/ubnt-letsencrypt) por [Jesse Miller](https://github.com/j-c-m)

```sh
configure
set system static-host-mapping host-name <hostname> inet <ip-of-edgerouter>
commit; save
```

**Crear certificado**

```sh
$ mkcert <ip-of-edgerouter> <hostname>
cat <ip-of-edgerouter>+1-key.pem <ip-of-edgerouter>+1.pem > server.pem
```

**Copia de seguridad del archivo de certificado existente**

```sh
sudo cp /etc/lighttpd/server.pem /etc/lighttpd/.server-OLD.pem
exit
```

**Copie el nuevo archivo de certificado en la dirección del usuario de su router**

```sh
scp /path/to/server.pem <user>@<ip-of-edgerouter>:/home/<user>/server.pem
```

**Copiar el nuevo archivo de certificado desde la dirección del usuario y habilitar el certificado**

```sh
sudo cp /home/<user>/server.pem /etc/lighttpd/server.pem
# Kill webserver service by PID
sudo kill -SIGINT $(cat /var/run/lighttpd.pid)
# Start webserver
sudo /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
exit
```

**Comprueba tu conexión con curl**  
Si se hace correctamente, una forma de comprobarlo es utilizar curl. Si obtiene una redirección a un puerto de protocolo SSL, es decir, 443, el certificado está instalado correctamente en su router.

```sh
$ curl -I http:/<ip-of-edgerouter>
HTTP/1.1 301 Moved Permanently
Location: https://<ip-of-edgerouter>:443/
Date: Sun, 11 Jan 2015 07:46:13 GMT
Server: Server
```


## Añadir listas de seguridad al firewall

#### Cree el grupo y agregue una regla de firewall para eliminar ese grupo:
```
set firewall group network-group SPAMHAUS_DROP
```
```
commit
```

```
set firewall name WAN_IN rule 10 action drop
set firewall name WAN_IN rule 10 source group network-group SPAMHAUS_DROP
set firewall name WAN_IN rule 10 description "networks to drop from spamhaus.org list"
```
```
commit; save
```

#### Crear y Añadir el script /config/scripts/post-config.d/update-spamhaus

```bash
#!/bin/bash

NETGROUP="SPAMHAUS_DROP"
TMPFILE=/tmp/spamhaus-block-$$.tmp
TMPFILE2=/tmp/temp-spamhaus-block-$$.tmp

clean_up ()
{
     /sbin/ipset --destroy $NEWGROUP
     /bin/rm $TMPFILE $TMPFILE2
}

>$TMPFILE>$TMPFILE2
/usr/bin/curl -s http://www.spamhaus.org/drop/drop.txt >> $TMPFILE2
/usr/bin/curl -s http://www.spamhaus.org/drop/edrop.txt >> $TMPFILE2

# Filter out comments and remove empty lines and duplicates
/bin/grep '^[0-9]' $TMPFILE2 | /bin/sed -e 's/;.*//' -e 's/[ \t]*$//' | /usr/bin/uniq > $TMPFILE


/sbin/ipset -L $NETGROUP > /dev/null 2>&1
if [ "$?" != 0 ]; then
   logger -i -s -- "firewall network group $NETGROUP doesn't exist yet"
   clean_up
   exit 1
fi

NEWGROUP=$NETGROUP-$$
/sbin/ipset --create $NEWGROUP nethash
if [ "$?" != 0 ]; then
  clean_up
  logger -i -s -- "There was an error trying to create temporary set"
  exit 1
fi

count=0;
for i in `cat $TMPFILE`;
do
  /sbin/ipset -q -A $NEWGROUP $i
  if [ "$?" != 0 ]; then
     logger -i -s -- "There was an error trying to add $i"
     clean_up
     exit 1
  fi
  let "count++"
done

/sbin/ipset --swap $NEWGROUP $NETGROUP
if [ "$?" != 0 ]; then
  logger -i -s -- "There was an error trying to swap temporary set"
  clean_up
  exit 1
fi

# Clean up temporary files and temp iptables group
clean_up

logger -i -s -- "added $count entries to $NETGROUP"
exit 0
```

#### Hazlo ejecutable:
```
chmod +x /config/scripts/post-config.d/update-spamhaus
```

EJECUTAR:
```
/config/scripts/post-config.d/update-spamhaus
```
- Resultado:
Added 561 entries to SPAMHAUS_DROP

<p>EDIT: Probablemente sería aún mejor poner el script en /config/scripts/post-config.d porque después de un reinicio el grupo de firewall volverá a estar vacío, pero si el script está en ese directorio, se ejecutará automáticamente después del arranque</p>
  
  
## PROGRAMAR TAREA:

* OPCIÓN 1:
####  Este es el programador de tareas, tengo el mío configurado para ejecutar un cron diario cada 12h:
<code>user@er4# set system task-scheduler {task update_spamhaus {crontab-spec "00 12 * * *"ejecutable {path /config/scripts/post-config.d/update-spamhaus}</code>

* OPCIÓN 2:
####  Simplemente agregue el script al programador de tareas tal como está:
<code>user@er4# set system task-scheduler task Update-SpamHaus executable path /config/scripts/post-config.d/update-spamhaus</code>
<code>user@er# set system task-scheduler task Update-SpamHaus interval 24h</code>

* OPCIÓN 3:
O coloque su configuración para que sobreviva a una actualización cada 24h:
<code>user@er4# set system task-scheduler SPAMHAUS {crontab-spec "00 24 * * *" executable {path /config/scripts/post-config.d/update-spamhaus}}</code>

--> Despues vemos las tareas
<code>user@er4# show system task-scheduler</code>



####  He estado usando la lista de bloqueo de spamhaus durante más de un año.
<p>Hace un par de meses decidí los siguientes dos cambios en mi firewall:</p>

* Coloque la regla spamhaus en primer lugar en WAN_IN y WAN_LOCAL (es decir, antes de la regla de permiso para conexiones establecidas y relacionadas). Esto es para evitar la situación "rara" de que un host interno (por ejemplo, infectado con malware) de alguna manera establezca una conexión con un host listado de spamhaus, dando la oportunidad de usar la conexión establecida para fines de spam.
* Ponga la regla de spamhaus en WAN_OUT, otra vez antes que cualquier otra cosa.
* Hoy noté en mis registros que el WAN_OUT coincidió (y rechazó) con el tráfico saliente a la dirección IP 185.3.135.146 (búsqueda de spamhaus aquí, listado desde el 29/2/2016). Este tráfico se originó en el cliente bittorrent que se ejecuta en mi NAS. No sé si los spammers usan bittorrent para infiltrarse en hosts posiblemente vulnerables, pero lo considero como un paso de protección adicional que funcionó.


## REVISIÓN

* Listar
sudo /sbin/ipset list
* Utilice este comando a través de la CLI para ver las entradas:
show firewall group SPAMHAUS_DROP
* Despues vemos las tareas
show system task-scheduler



EJEMPLO:
```sh
    name WAN_IN {
        default-action drop
        description "WAN to internal"
        enable-default-log
        rule 10 {
            action drop
            description "Networks to drop from spamhaus.org list"
            log enable
            source {
                group {
                    network-group SPAMHAUS_DROP
                }
            }
        }
.........................
.........................

 name WAN_LOCAL {
        default-action drop
        description "WAN to router"
        enable-default-log
        rule 10 {
            action drop
            description "Networks to drop from spamhaus.org list"
            log enable
            source {
                group {
                    network-group SPAMHAUS_DROP
                }
            }
        }
.........................
.........................

    name WAN_OUT {
        default-action accept
        description "WAN OUT firewall rules"
        rule 10 {
            action reject
            description "Networks to drop from spamhaus.org list"
            destination {
                group {
                    network-group SPAMHAUS_DROP
                }
            }
            log enable
        }
```