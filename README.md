# Wiki Ubiquiti
Una colección de mejoras para los dispositivos basados en EdgeMax.

<p align="center">
    <a href="https://pi-hole.net/">
        <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/UbiquitiXEdgeMax.jpg" alt="UbiquitiXEdgeMax">
    </a>
    <br>
    <strong>Ubiquiti networks x EdgeMax©</strong>
</p>
<!-- markdownlint-enable MD033 -->

## <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/UbiquitiConf.png" alt="Ubiquiti edgemax" width="40"/></a> Configuración inicial del EdgeRouter 4:

<div class="article-notice-box box--purple">
<div id="gui-icon" class="note-table__cell-icon"></div>
<div class="note-table--text">
<div class="node--head"><span class="node--head-title"> <strong>GUI:</strong> Access the EdgeRouter Web UI</span>.</div>

#### Actualizar EdgeRouter
Antes de Realizar cualquier cambio o configuración en los equipos Ubiquiti EdgeMax debe contar con la última versión del Firmware.
https://www.ui.com/download/edgemax/


#### Acceso a la interfaz de configuración EdgeOS
<details>
    <summary>Opción 1:</summary>

## Instrucciones de uso con IP ESTÁTICA:

1. Conecte un cable Ethernet desde el puerto Ethernet del ordenador al puerto eth0 del EdgeRouter.
2. Configure el adaptador de Ethernet en su sistema host con una dirección IP estática en la subred  <code>192.168.1.x</code>.
3. Inicie el explorador web. Escriba <code>https://192.168.1.1</code> en la barra de direcciones. Pulse Intro (PC) o Retorno (Mac).
4. Introduzca ubnt en los campos de nombre de usuario y contraseña. Lea el acuerdo de licencia de Ubiquiti y marque la casilla junto a I agree to the terms of this License Agreement (Acepto los términos de este acuerdo de licencia) para aceptarlo. Haga clic en Login (Inicio de sesión).

&nbsp;
</details>
&nbsp;

<details>
    <summary>Opción 2:</summary>

## Instrucciones de uso con DHCP:

1. Conecte un cable Ethernet de eth1 en el EdgeRouter a un segmento de LAN que ya tiene un servidor DHCP.
2. Para comprobar la dirección IP del EdgeRouter, utilice uno de los métodos siguientes:
<ul>2.1 Configure el servidor DHCP para que proporcione una dirección IP específica al EdgeRouter en función de su dirección MAC (en la etiqueta).</ul>
<ul>2.2 Deje que el EdgeRouter obtenga una dirección IP y luego compruebe el servidor DHCP para ver qué dirección IP se asignó.</ul>
3. Inicie el explorador web. Introduzca la dirección IP correcta en el campo de dirección. Pulse Intro (PC) o Retorno (Mac).
4. Introduzca ubnt en los campos de nombre de usuario y contraseña. Lea el acuerdo de licencia de Ubiquiti y marque la casilla junto a I agree to the terms of this License Agreement (Acepto los términos de este acuerdo de licencia) para aceptarlo. Haga clic en Login (Inicio de sesión).

&nbsp;
</details>
&nbsp;

#### Gestión de UISP
Puede administrar el dispositivo mediante el UISP, que le permite configurar, supervisar, actualizar y realizar copias de seguridad de sus dispositivos a través de una sola aplicación. Para empezar, vaya a <a href="uisp.ui.com">

### Comandos básicos
<ul><code>commit: para activar los cambios.</code></ul>
<ul><code>save: para almacenar la configuración "activa" en la configuración de inicio.</code></ul>
<ul><code>compare: Para ver qué cambios se han realizado en la configuración.</code></ul>
<ul><code>configure: modo configuración.</code></ul>
<ul><code>show: mostrar</code></ul>
<ul><code>set: establecer configuración.</code></ul>
<ul><code>edit: Cambiat el nivel de edición.</code></ul>
<ul><code>up:</code></ul>
<ul><code>top:</code></ul>
<ul><code>discard: para deshacer los cambios no confirmados</code></ul>
<ul><code>copy:</code></ul>
<ul><code>rename:</code></ul>
<ul><code>load: cargar configuración.</code></ul>

#### Inicie sesión en el router y añada un nuevo usuario

```sh
configure
set system login user <user> authentication plaintext-password <secret>
set system login user <user> level admin
commit; save
```

#### Habilitar funciones de rendimiento
Para ER-X,ER-X-SPF,EP-R6
```
configure
set system offload hwnat enable
set system offload ipsec enable
commit; save
```

Para todos los demás modelos de ER
```
configure
set system offload ipv4 forwarding enable
set system offload ipv4 gre enable
set system offload ipv4 pppoe enable
set system offload ipv4 vlan enable
set system offload ipv6 forwarding enable
set system offload ipv6 pppoe enable
set system offload ipv6 vlan enable
set system offload ipsec enable
commit; save
```

## <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/hardening.png" alt="Ubiquiti edgemax" width="40"/></a> Hardening del dispositivo

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


### <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/Icon-Firewall.png" alt="Ubiquiti edgemax" width="40"/></a> Firewall e interfaces Edgerouter

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

## <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/block.png" alt="Ubiquiti edgemax" width="40"/></a> Añadir listas de seguridad al firewall

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
<ul><blockquote class="is-info"><p>EDIT: Probablemente sería aún mejor poner el script en <code>/config/scripts/post-config.d</code> porque después de un reinicio el grupo de firewall volverá a estar vacío, pero si el script está en ese directorio, se ejecutará automáticamente después del arranque.</p></blockquote></ul>

```sh
sudo vi /config/scripts/post-config.d/update-spamhaus
```

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
Resultado:
<ul>Added 561 entries to SPAMHAUS_DROP</ul>

  
### PROGRAMAR TAREA:

OPCIÓN 1:
####  Este es el programador de tareas, tengo el mío configurado para ejecutar un cron diario cada 12h:
<ul><code>user@er4# set system task-scheduler {task update_spamhaus {crontab-spec "00 12 * * *"ejecutable {path /config/scripts/post-config.d/update-spamhaus}</code></ul>

OPCIÓN 2:
####  Simplemente agregue el script al programador de tareas tal como está:
<ul><code>user@er4# set system task-scheduler task Update-SpamHaus executable path /config/scripts/post-config.d/update-spamhaus</code>
<code>user@er# set system task-scheduler task Update-SpamHaus interval 24h</code></ul>

OPCIÓN 3:
O coloque su configuración para que sobreviva a una actualización cada 24h:
<ul><code>user@er4# set system task-scheduler SPAMHAUS {crontab-spec "00 24 * * *" executable {path /config/scripts/post-config.d/update-spamhaus}}</code></ul>

--> Despues vemos las tareas
<ul><code>user@er4# show system task-scheduler</code></ul>



####  He estado usando la lista de bloqueo de spamhaus durante más de un año.
<p>Hace un par de meses decidí los siguientes dos cambios en mi firewall:</p>

- Coloque la regla spamhaus en primer lugar en WAN_IN y WAN_LOCAL (es decir, antes de la regla de permiso para conexiones establecidas y relacionadas). Esto es para evitar la situación "rara" de que un host interno (por ejemplo, infectado con malware) de alguna manera establezca una conexión con un host listado de spamhaus, dando la oportunidad de usar la conexión establecida para fines de spam.
- Ponga la regla de spamhaus en WAN_OUT, otra vez antes que cualquier otra cosa.
- Hoy noté en mis registros que el WAN_OUT coincidió (y rechazó) con el tráfico saliente a la dirección IP 185.3.135.146 (búsqueda de spamhaus aquí, listado desde el 29/2/2016). Este tráfico se originó en el cliente bittorrent que se ejecuta en mi NAS. No sé si los spammers usan bittorrent para infiltrarse en hosts posiblemente vulnerables, pero lo considero como un paso de protección adicional que funcionó.


## REVISIÓN
<p>Listar</p>
<ul><code>sudo /sbin/ipset list</code></ul>
<p>Utilice este comando a través de la CLI para ver las entradas:</p>
<ul><code>show firewall group SPAMHAUS_DROP</code></ul>
<p>Despues vemos las tareas</p>
<ul><code>show system task-scheduler</code></ul>



### EJEMPLO:
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


## <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/icon-certificate.png" alt="Ubiquiti edgemax" width="40"/></a> POSIBILIDAD DE AÑADIR UN CERTIFICADO A LOCALHOST
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
