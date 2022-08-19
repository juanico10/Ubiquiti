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

### Realización de un Hardware o Software Reset
El EdgeRouter se puede restablecer a los valores predeterminados de fábrica utilizando un hardware o software método de restablecimiento

Hardware Reset: Borra todos los archivos de configuración y del sistema, restableciendo el dispositivo al estado predeterminado de fábrica.
Software Reset: Solo borra la configuración y deja intactos los demás archivos del sistema.

<strong><font style="vertical-align: inherit;">ATENCIÓN: </font></strong> Los de hardware métodos de reinicio a continuación borrarán todos los archivos de configuración y del sistema.

<details>
    <summary>Realización de un restablecimiento de hardware :</summary>

## Instrucciones de uso para realizar reset button:

1. Verifique que EdgeRouter esté completamente iniciado. 
2. Mantenga presionado el reinicio.
3. Los LED del puerto comenzarán a encenderse en secuencia, comenzando por el puerto 1 y terminando en el último puerto.
4. Continúe presionando el botón de reinicio durante aproximadamente 10 segundos hasta que el LED del puerto 1 se encienda nuevamente.
5. Suelte el botón de reinicio.
6. El EdgeRouter se reiniciará.
7. Espere a que se complete el reinicio.
8. Conéctese al eth0 y administre el dispositivo abriendo un navegador y navegando a la <code>https://192.168.1.1</code> dirección IP predeterminada.

## Instrucciones de uso para realizar un Power On Reset:

1. Desconecte el cable de alimentación del EdgeRouter.
2. Mientras vuelve a conectar el cable de alimentación al EdgeRouter, mantenga presionado el reinicio .
3. Los LED del puerto comenzarán a encenderse en secuencia, comenzando por el puerto 1 y terminando en el último puerto.
4. Continúe presionando el botón de reinicio durante aproximadamente 10 segundos hasta que el LED del puerto 1 se encienda nuevamente.
5. Suelte el botón de reinicio.
6. El EdgeRouter se reiniciará.
7. Espere a que se complete el reinicio.
8. Conéctese al eth0 y administre el dispositivo abriendo un navegador y navegando a la <code>https://192.168.1.1</code> dirección IP predeterminada.

&nbsp;
</details>
&nbsp;


<details>
    <summary>Realización de un reinicio de software:</summary>

## Instrucciones de uso con GUI:

0. Acceder a la interfaz web de EdgeRouter. acceda a la interfaz de usuario web de EdgeRouter
1. Navegue a la Sistema en la parte inferior izquierda de la interfaz de usuario web.
2. Restablezca la configuración a los valores predeterminados presionando el Restablecer a los valores predeterminados en la Restablecer  predeterminados .
3. El EdgeRouter solicitará que se reinicie el dispositivo para completar el restablecimiento.
4. Espere a que se complete el reinicio.
5. Conéctese al eth0 y administre el dispositivo abriendo un navegador y navegando a la <code>https://192.168.1.1</code> dirección IP predeterminada 

## Instrucciones de uso con CLI:

0. acceda a la interfaz de línea de comandos de EdgeRouter.
1. Sobrescriba el archivo de inicio actual ( config.boot ) con el archivo de inicio predeterminado ( config.boot.default ).
<ul><code>sudo cp /opt/vyatta/etc/config.boot.default /config/config.boot</code></ul>
Haga clic para copiar
2. Reinicie EdgeRouter.
<ul><code>reboot</code></ul>
<ul><code>Proceed with reboot? [confirm]</code></ul>
3. Espere a que se complete el reinicio.
4. Conéctese al eth0 y administre el dispositivo abriendo un navegador y navegando a la <code>https://192.168.1.1</code> dirección IP predeterminada  

&nbsp;
</details>
&nbsp;

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
commit ; save
```

#### Habilitar funciones de rendimiento
Para ER-X,ER-X-SPF,EP-R6
```
configure
set system offload hwnat enable
set system offload ipsec enable
commit ; save
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
commit ; save
```

## <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/hardening.png" alt="Ubiquiti edgemax" width="40"/></a> Hardening del dispositivo

#### Remover default user

```sh
configure
delete system login user ubnt
commit ; save
```

#### Añadir una clave ssh pública a EdgeRouter

```sh
$ scp ~/.ssh/id_rsa.pub <ip-of-edgerouter>:/tmp
```

```sh
configure  
loadkey <user> /tmp/id_rsa.pub  
sudo chown -R <user> /home/<user>
commit ; save
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
commit ; save
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
commit ; save
```


### <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/Icon-Firewall.png" alt="Ubiquiti edgemax" width="40"/></a> Firewall e interfaces Edgerouter

#### Firewall

```sh
configure

set firewall name WAN_IN default-action drop
set firewall name WAN_IN description 'WAN to internal'
set firewall name WAN_IN rule 10 action accept
set firewall name WAN_IN rule 10 description 'Allow established/related'
set firewall name WAN_IN rule 10 state established enable
set firewall name WAN_IN rule 10 state related enable
set firewall name WAN_IN rule 20 action drop
set firewall name WAN_IN rule 20 description 'Drop invalid state'
set firewall name WAN_IN rule 20 state invalid enable

set firewall name WAN_LOCAL default-action drop
set firewall name WAN_LOCAL description 'WAN to router'
set firewall name WAN_LOCAL rule 10 action accept
set firewall name WAN_LOCAL rule 10 description 'Allow established/related'
set firewall name WAN_LOCAL rule 10 state established enable
set firewall name WAN_LOCAL rule 10 state related enable
set firewall name WAN_LOCAL rule 20 action drop
set firewall name WAN_LOCAL rule 20 description 'Drop invalid state'
set firewall name WAN_LOCAL rule 20 state invalid enable

set interfaces ethernet eth0 firewall in name WAN_IN
set interfaces ethernet eth0 firewall local name WAN_LOCAL

commit ; save
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
commit ; save
```

#### LAN
**NOTE!** Asegúrate de cambiar el rando de la red a la de tu red y la interfaz a modificar
```sh
configure
set interfaces ethernet eth1 description LAN
set interfaces ethernet eth1 address 192.168.1.1/24
set service dhcp-server disabled false
set service dhcp-server shared-network-name LAN authoritative enable
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 default-router 192.168.1.1
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 dns-server 192.168.1.1
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 lease 86400
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 start 192.168.1.38 stop 192.168.1.243
set service dns forwarding listen-on eth3
commit ; save
```

#### Port Forwarding
Seleccione las interfaces WAN y LAN que se utilizarán para el reenvío de puertos.
- Pueden realizar el procedimiento mediante la GUI o mediante CLI.
Mediante CLI: Firewall / NAT > Port Forwarding

**NOTE!** Asegúrate de cambiar el rando de la red a la de tu red y la interfaz a modificar
```sh
configure

set port-forward auto-firewall enable
set port-forward hairpin-nat enable
set port-forward wan-interface eth0
set port-forward lan-interface eth1

set port-forward rule 1 description https
set port-forward rule 1 forward-to address 192.168.1.10
set port-forward rule 1 forward-to port 443
set port-forward rule 1 original-port 443
set port-forward rule 1 protocol tcp

commit ; save
```

#### DNS DINAMICO
<div class="flex page-col-content xs12 lg9 xl10"><!----><!----><div class="contents"><div><p>Si tienes algún subdominio registrador en <a href="https://www.duckdns.org/" class="is-external-link">www.duckdns.org</a> y quieres asignárselo a tu <em>ER-X</em> puedes realizarlo de dos maneras, mediante el formulario web o la línea de comandos.</p><div></div><h2 id="obtener-nuestro-token-de-duckdns" class="toc-header"><a href="#obtener-nuestro-token-de-duckdns" class="toc-anchor">¶</a> Obtener nuestro <em>Token</em> de DuckDNS</h2><div></div><p>Durante el proceso de configuración es necesario disponer a mano del <em>Token</em> que nos identifica en el servicio <em>DuckDNS</em>. Puede que estés acostumbrado a identificarte mediante un <em>usuario</em> y <em>contraseña</em>, pero en este caso deberás hacerlo mediante un <em>Token</em>.</p><div></div><p>Para obtenerlo simplemente deberemos iniciar sesión vía web en nuestra cuenta y justo en la parte superior podremos ver nuestro <em>token</em>, un conjunto de números y letras.</p><div></div><p><img alt="er-x_dudckdns_3.png" src="/imagenes/er-x_dudckdns_3.png" class="align-center"></p><div></div><p>Hay que tener a mano dicho dato ya que lo vamos a necesitar en el proceso de configuración.</p><div></div><h2 id="mediante-la-web" class="toc-header"><a href="#mediante-la-web" class="toc-anchor">¶</a> Mediante la Web</h2><div></div><p>Estando dentro de la web de gestión entramos en la pestaña <code>Service</code> y a continuación en <code>DNS</code>. Por ultimo en la sección <em>Dynamic DNS</em> pulsamos el botón <code>+ Add DDNS Interface</code>.</p><div></div><p><img alt="er-x_dudckdns_1.png" src="/imagenes/er-x_dudckdns_1.png" class="align-center"></p><div></div><p>Se cargará un formulario vació que deberemos rellenar con los datos adecuados:</p><div></div><ul><li><p><strong>Interface:</strong> Aquí hay que seleccionar la interfaz en la que está configurada nuestra IP pública.</p></li> <li><p><strong>Service:</strong> En el menú desplegable hay varios servicios ya pre-configurados, pero entre ellos al no estar <em>DuckDNS</em> optamos por la opción <em>custom</em>.</p></li> <li><p><strong>Hostname:</strong> Aquí hay que meter el subdominio <em>DuckDNS</em> que queremos asignar a nuestro router. Solamente el subdominio, no hace falta meter <em>.duckdns.org</em>.</p></li> <li><p><strong>Login:</strong> poniendo <em>nouser</em> servirá, ya que nos identificaremos mediante nuestro <em>Token</em>.</p></li> <li><p><strong>Password:</strong> Aquí deberemos introducir el <em>Token</em> de nuestra cuenta.</p></li> <li><p><strong>Protocol:</strong> Seleccionamos el protocolo <em>dyndns2</em>.</p></li> <li><p><strong>Server:</strong> por último metemos la <em>url</em> del servidor de <em>DuckDNS</em>, <em><a href="http://www.duckdns.org" class="is-external-link">www.duckdns.org</a></em>.</p></li></ul><div></div><p><img alt="er-x_dudckdns_2.png" src="/imagenes/er-x_dudckdns_2.png" class="align-center"></p><div></div><p>Para terminar pulsamos en <code>Apply</code> para guardar todo lo que hemos metido.</p><div></div><h2 id="a-través-de-la-línea-de-comandos-cli" class="toc-header"><a href="#a-través-de-la-línea-de-comandos-cli" class="toc-anchor">¶</a> A través de la línea de comandos (CLI)</h2><div></div><p>Esto podemos realizarlo conectando al router mediante el protocolo <em>SSH</em> o usando el intérprete <em>CLI</em> incorporado en la propia web de gestión. En todo caso ya sea mediante un método u otro, deberemos iniciar sesión utilizando las mismas credenciales que usamos para acceder vía web.</p><div></div><p><img alt="er-x_dudckdns_4.png" src="/imagenes/er-x_dudckdns_4.png" class="align-center"></p><div></div><p><img alt="er-x_dudckdns_4.png" src="/imagenes/er-x_dudckdns_5.png" class="align-center"></p><div></div><p>Ahora deberemos entrar en modo configuración. Para ello escribimos el siguiente comando y pulsares <em>Enter</em>.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash">configure<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Se la misma manera que en la web de gestión, deberemos de indicar la <em>Interfaz</em> en la que tenemos nuestra IP pública. Para ello sustituye la palabra <em>INTERFAZ</em> en el siguiente comando y los siguientes también.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash"><span class="token builtin class-name">set</span> <span class="token function">service</span> dns dynamic interface INTERFAZ <span class="token function">service</span> custom-duckdns<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Le indicamos el <em>subdominio</em> <em>DuskDNS</em> que le asignaremos al router.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash"><span class="token builtin class-name">set</span> <span class="token function">service</span> dns dynamic interface INTERFAZ <span class="token function">service</span> custom-duckdns host-name SUBDMIONIO<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Al identificarnos mediante un <em>Token</em> no es necesario un usuario, de ahí el <em>nouser</em>.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash"><span class="token builtin class-name">set</span> <span class="token function">service</span> dns dynamic interface INTERFAZ <span class="token function">service</span> custom-duckdns login nouser<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Ahora le pasaremos en <em>Token</em> de nuestra cuenta sustituyendo la palabra <em>TOKEN</em> del siguiente comando.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash"><span class="token builtin class-name">set</span> <span class="token function">service</span> dns dynamic interface INTERFAZ <span class="token function">service</span> custom-duckdns password TOKEN<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Definimos el protocolo a utilizar metiendo el siguiente comando tal como está. Solamente cambiando la <em>INTERFAZ</em> como en el resto de comandos.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash"><span class="token builtin class-name">set</span> <span class="token function">service</span> dns dynamic interface INTERFAZ <span class="token function">service</span> custom-duckdns protocol dyndns2<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Por último, concretamos el servidor al que le deberemos indicar nuestros cambios de IP.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash"><span class="token builtin class-name">set</span> <span class="token function">service</span> dns dynamic interface INTERFAZ <span class="token function">service</span> custom-duckdns server www.duckdns.org<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Para guardar los cambios y salir del modo <em>configuración</em> metemos los siguientes comandos.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash">commit
save
<span class="token builtin class-name">exit</span><span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Ahora ya tenemos todo configurado.</p><div></div><p>Periódicamente el router ira actualizando nuestra IP pública en el <em>DNS</em> de <em>DuckDNS</em>. En caso de querer forzar la actualización, se puede realizar lanzando el siguiente comando.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash">update dns dynamic interface INTERFAZ<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Si hemos realizado bien todos los pasos anteriores, ejecutando el siguiente comando veremos si todo está funcionando como debería.</p><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash">show dns dynamic status<span aria-hidden="true" class="line-numbers-rows"><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><div class="code-toolbar"><pre class="prismjs line-numbers language-bash"><code class="language-bash">interface    <span class="token builtin class-name">:</span> INTERFAZ
<span class="token function">ip</span> address   <span class="token builtin class-name">:</span> xxx.xxx.xxx.xxx
host-name    <span class="token builtin class-name">:</span> SUBDOMINIO
last update  <span class="token builtin class-name">:</span> Tue Sep <span class="token number">29</span> <span class="token number">22</span>:28:09 <span class="token number">2020</span>
update-status: good<span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span></span></code></pre><div class="toolbar"><div class="toolbar-item"><button>Copy</button></div></div></div><div></div><p>Si en el apartado <code>update-status:</code> vemos que aparece <code>good</code> es que todo está funcionando perfectamente.</p><div></div><hr><div></div><p>Fuente: <a href="https://loganmarchione.com/2017/04/duckdns-on-edgerouter/" class="is-external-link">https://loganmarchione.com/2017/04/duckdns-on-edgerouter/</a></p><div></div></div></div><!----></div>

## <a title="Icon config" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/block.png" alt="Ubiquiti edgemax" width="40"/></a> Añadir listas de seguridad al firewall

#### Cree el grupo y agregue una regla de firewall para eliminar ese grupo:
```
set firewall group network-group SPAMHAUS_DROP
commit
```

```
set firewall name WAN_IN rule 10 action drop
set firewall name WAN_IN rule 10 source group network-group SPAMHAUS_DROP
set firewall name WAN_IN rule 10 description "networks to drop from spamhaus.org list"
commit ; save
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
commit ; save
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
