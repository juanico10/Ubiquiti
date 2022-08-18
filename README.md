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