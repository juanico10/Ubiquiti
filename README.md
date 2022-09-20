# Wiki Ubiquiti
Una colección de mejoras para los dispositivos basados en EdgeMax.

<p align="center">
    <a href="https://www.ui.com/">
        <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/UbiquitiXEdgeMax.jpg" alt="UbiquitiXEdgeMax">
    </a>
    <br>
    <strong>Ubiquiti networks x EdgeMax©</strong>
</p>
<!-- markdownlint-enable MD033 -->

---
<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/> Recordar que los pasos aquí expuestos son orientativos.<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/>
<p>Recomiendo su lectura y compresión antes de aplicarlo sobre un entorno de producción.</p>

## Acceso a la CLI y comandos básicos
Los lectores aprenderán cómo conectarse y configurar un EdgeRouter por primera vez. Hay muchos entornos diferentes en los que es posible que sea necesario realizar ajustes específicos. Este artículo muestra un escenario de instalación común, pero no es necesario aplicarlo en todos los entornos de red. 

### Comandos básicos
<ul><code>commit:</code>para activar los cambios.</ul>
<ul><code>save:</code> para almacenar la configuración "activa" en la configuración de inicio.</ul>
<ul><code>compare:</code> Para ver qué cambios se han realizado en la configuración.</ul>
<ul><code>configure:</code> modo configuración.</ul>
<ul><code>show:</code> mostrar -> Ejemplo: show firewall</ul>
<ul><code>set:</code> establecer configuración. -> Ejemplo: set firewall name TEST default-action drop</ul>
<ul><code>edit:</code> Para crear la misma acción y reducir la cantidad de repeticiones en la sintaxis completa. -> Ejemplo: edit firewall name TEST</ul>
<ul><code>up:</code> Para subir un nivel de edición -> Ejemplo: volver al set de configuración anterior</ul>
<ul><code>top:</code> Para volver al nivel de edición superior -> Ejemplo: Volver al ser de configuración incial</ul>
<ul><code>discard:</code> para deshacer los cambios no confirmados</ul>
<ul><code>copy:</code> Para crear una nueva acción existente -> Ejemplo: copy name WAN1_LOCAL to name WAN2_LOCAL</ul>
<ul><code>rename:</code> Para cambiar el nombre de la nueva acción -> Ejemplo: rename name WAN2_LOCAL to name WAN2_IN</ul>
<ul><code>load:</code> cargar configuración.</ul>
<ul><code>? o tecla de tab:</code> para mostrar opciones para el nivel de edición especificado</ul>

#### Comandos básicos de VI
<ul><code>wq</code>Guardar y salir.</ul>
<ul><code>:q!</code>Salir sin guardar.</ul>
<ul><code>u</code>Deshace última acción.</ul>
<ul><code>i</code>modo inserción por la izquierda.</ul>
<ul><code>a</code>modo inserción por la derecha.</ul>
<ul><code>dd</code>Elimina la línea actual.</ul>
<ul><code>p</code>Copia la palabra actual.</ul>
<ul><code>A</code>Pone le puntero al final de la línea.</ul>
<ul><code>w</code>Salta de palabra en palabra.</ul>
<ul><code>D</code>Borra desde el puntero hacia el final de la línea.</ul>
<ul><code>o</code>Agrera una línea debajo de la actual.</ul>
<ul><code>O</code>Agrega una línea encima de la actual.</ul>
<ul><code>P</code>Copia la palabra pegada.</ul>
<ul><code>gw</code>Copia la palabra actual.</ul>
<ul><code>help</code>Ayuda.</ul>

### Acceso a la GUI
Mediante un navegador, accedemos a <code>https://192.168.1.1</code>. Cargara una web donde deberemos introducir unas credenciales. En este caso las que vienen de fábrica.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/gui/web.png" alt="web.png"></p>

La contraseña por defecto:
<ul><code>Usuario:</code> ubnt</ul>
<ul><code>Contraseña:</code> ubnt</ul>
  
Una vez introducidas las credenciales, se cargará la web de gestión del EdgeRouter.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/gui/web1.png" alt="web1.png"></p>

#### Instrucciones de uso con Putty o similar:
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/CLI1.png" alt="CLI1"></p>

#### Instrucciones de uso con GUI:
0. Acceda a la interfaz de usuario web de EdgeRouter
1. Navegue a la parte superior derecha de la interfaz de usuario web.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/CLI2.png" alt="CLI2"></p>

#### Solucionar problema con certificado inválido
Cuando intentamos acceder vía web, nos indica que el certificado es inválido al ser autofirmado:
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/cert/cert0.PNG" alt="cert0"></p>

Para poder solucionar, debemos descargar el certificado del navegador. Nos descarga un archivo <code>.pem</code>:
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/cert/cert1.PNG" alt="cert1"></p>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/cert/cert2.PNG" alt="cert2"></p>

Una vez descargado tenemos que cambiar el <code>.pem</code> a <code>.crt</code> con OpenSSL:
```sh
openssl x509 -outform der -in <nombre_certificado>.pem -out <nombre_certificado>.crt
```
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/cert/cert3.PNG" alt="cert3"></p>

Despues de haber cambiado el formato, procedemos a instalar el certificado en la raiz de confianza:
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/cert/cert4.PNG" alt="cert4"></p>

Una vez importado el certificado y borrado las cookies, ya no nos indicará que el certificado no es de confianza.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/cert/cert5.PNG" alt="cert5"></p>

---
## <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/UbiquitiConf.png" alt="Ubiquiti edgemax" width="40"/> Configuración inicial del EdgeRouter:

### Realización de un Hardware o Software Reset
El EdgeRouter se puede restablecer a los valores predeterminados de fábrica utilizando un hardware o software método de restablecimiento

Hardware Reset: Borra todos los archivos de configuración y del sistema, restableciendo el dispositivo al estado predeterminado de fábrica.
Software Reset: Solo borra la configuración y deja intactos los demás archivos del sistema.

<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/> <strong><font style="vertical-align: inherit;">ATENCIÓN: </font></strong> Los métodos de reinicio de hardware a continuación borrarán todos los archivos de configuración y del sistema.

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
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/reset.gif" alt="reset.gif"></p>

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

0. Acceda a la interfaz de usuario web de EdgeRouter.
1. Navegue a la Sistema en la parte inferior izquierda de la interfaz de usuario web.
2. Restablezca la configuración a los valores predeterminados presionando el Restablecer a los valores predeterminados en la Restablecer  predeterminados .
3. El EdgeRouter solicitará que se reinicie el dispositivo para completar el restablecimiento.
4. Espere a que se complete el reinicio.
5. Conéctese al eth0 y administre el dispositivo abriendo un navegador y navegando a la <code>https://192.168.1.1</code> dirección IP predeterminada 

## Instrucciones de uso con CLI:

0. acceda a la interfaz de línea de comandos de EdgeRouter.
1. Sobrescriba el archivo de inicio actual (config.boot) con el archivo de inicio predeterminado (config.boot.default).
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

### Actualizar EdgeRouter
Antes de Realizar cualquier cambio o configuración en los equipos Ubiquiti EdgeMax debe contar con la última versión del Firmware.
<a title="download" href="https://www.ui.com/download/edgemax/"><img src="https://github.com/JuanRodenas/Duckdns/blob/main/files/down.png" alt="download" width="100" align="center" /></a>

Y seguir la guía que indica fabricante: <a href="https://help.ui.com/hc/en-us/articles/205146110-EdgeRouter-How-to-Upgrade-the-EdgeOS-Firmware">Cómo actualizar el firmware de EdgeOS</a>

### Acceso a la interfaz de configuración EdgeOS
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

### Gestión de UISP
Puede administrar el dispositivo mediante el UISP, que le permite configurar, supervisar, actualizar y realizar copias de seguridad de sus dispositivos a través de una sola aplicación.
1. Para empezar, vaya a <a href="https://help.ui.com/hc/en-us/articles/115012196527-UNMS-Installation-Guide">UISP - Guía de instalación </a>
2. Despues logarse en la web de UISP <a href="uisp.ui.com">uisp.ui.com</a>
3. Pueden utilizar la aplicación móvil, enlace de configuración: <a href="https://help.ui.com/hc/en-us/articles/115010608187-UISP-Mobile-App#2">UISP-Mobile-App</a>
- Android: <a href="https://play.google.com/store/apps/details?id=com.ubnt.umobile">Android</a>
- IOS: <a href="https://apps.apple.com/us/app/unms-mobile/id1183022489">IOS</a>

<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/> Cuidado con la opción cloud. Cuando creas la cuenta indica:
<ul><code>Entiendo que una consola en la nube de UISP gratuita requiere al menos 10 dispositivos Ubiquiti activos en total después del día 30 de la configuración.</code></ul>

### Inicie sesión en el router y añada un nuevo usuario

```sh
configure
set system login user <user> authentication plaintext-password <secret>
set system login user <user> level admin
commit ; save
```

### Habilitar funciones de rendimiento
Offloading se utiliza para ejecutar funciones del enrutador usando el hardware directamente, en lugar de un proceso de funciones de software.  El beneficio de la descarga en EdgeOS es un mayor rendimiento y rendimiento al no depender de la CPU para las decisiones de reenvío. Enlace a la web oficial de Ubiquiti: <a href="https://help.ui.com/hc/en-us/articles/115006567467-EdgeRouter-Hardware-Offloading">EdgeRouter-Hardware-Offloading</a></p>
**UTILIZAR CON CUIDADO**.

Para ER-X,ER-X-SPF,EP-R6
```
configure
set system offload hwnat enable
set system offload ipsec enable
commit ; save
```

Para todos los demás modelos de Edgerouter
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

---
## <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/hardening.png" alt="Ubiquiti edgemax" width="40"/> Hardening del dispositivo

### Remover default user y crear un usuario
Antes de eliminar el usuario por defecto, crear un usuario, en la GUI en la pestaña USERS o por CLI:
```sh
set system login user <user>
set system login user <user> level admin
set system login user <user> authentication plaintext-password <contraseña>
set system login user <user> full-name <Nombre>
commit ; save
```
<sup>La contraseña se encripta una vez introducida en texto plano</sup>

Despues eliminarmos el usuario por defecto
```sh
configure
delete system login user ubnt
commit ; save
```
**PD:** Si creas un usuario como operador, no tiene acceso por ssh.
```sh
This account is currently not available.
Connection to 192.168.1.1 closed.
```

### Añadir una clave ssh pública a EdgeRouter
Para poder generar una clave pública hay muchas opciones, pero os recomiendo con Putty.
<p>Si no lo conocen, os dejo el tutorial: <a href="https://www.hostinger.es/tutoriales/llaves-ssh#Paso_2_-_Genera_un_par_de_SSH_key">Generar SSH Keys (Llaves SSH) en PuTTY</a></p>

```sh
$ scp ~/.ssh/id_rsa.pub <ip-of-edgerouter>:/tmp
```
<ul><code>Pueden utilizar Filezilla o similar para enviar el archivo.</code></ul>

Accedemos al equipo y configuramos la clave pública generada:
```sh
configure  
loadkey <user> /tmp/id_rsa.pub  
sudo chown -R <user> /home/<user>
commit ; save
```

### Comprobación de acceso
<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/>  Asegúrate de que puedes acceder con tu clave pública antes de salir de la sesión SSH actual.
Probamos acceso sin salir de la sesión SSH por si tienes que hacer un rollback:
```sh
$ ssh <user>@<ip-of-edgerouter>
exit
```

### Desactivar la autenticación de contraseñas en texto plano
Si puede iniciar sesión con éxito en el EdgeRouter, un paso para reforzar la seguridad de su EdgeRouter es eliminar la opción de utilizar una contraseña de texto simple.  
<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/>  Asegúrate de que puedes acceder con tu clave pública antes de desactivar la autenticación en texto plano.

```sh
configure
set service ssh disable-password-authentication
commit ; save
```
### Asegurar acceso a la GUI y ssh
Pueden asegurar el acceso al ssh o gui con vuestro rango de IPs, es opcional, pero seguro.
(opcional)
```sh
configure
set service gui listen-address <lan ip address/range>
set service ssh listen-address <lan ip address/range>
commit ; save
```

Recomendado, cambiar el puerto de ssh y habilitar V2
```sh
configure
set service ubnt-discover disable
set service ssh protocol-version v2
set service ssh port <port>
delete service telnet
commit ; save
```

---
## <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/Icon-Firewall.png" alt="Ubiquiti edgemax" width="40"/> Firewall e interfaces Edgerouter

### Firewall
Aquí viene la parte más difícil. Si anteriormente no te has peleado con un Firewall algunos conceptos te serán extraños, pero intentare explicar cada paso con algún ejemplo, haciéndolo mas fácil de entender.
Configuración básica del firewall:

Asignar la interfaz de la WAN que vayan a utilizar:
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

#### TIPOS DE REGLAS
Para poder añadir una regla, deben saber que hay 3 WAN en Ubiquiti:
- `WAN_IN` = La entrada de la WAN
- `WAN_LOCAL` = Para el tráfico hacia el router desde la WAN
- `WAN_OUT` = La salida de la WAN

Las reglas se añaden en las `WAN`, dependiendo del sentido que queramos hacer.
Si desea bloquear el entrante y saliente, debemos de añadir en la IN u OUT, y en el LOCAL para bloquear los accesos hacia el sentido del router.

Ejemplo para una `WAN`con pppoe,
- Indicamos la interfaz a la WAN_IN la pppoe en modo `IN`
- Indicamos la interfaz a la WAN_LOCAL la pppoe en modo `LOCAL`
- Indicamos la interfaz a la WAN_IN la pppoe en modo `OUT`.
    - <sup>No olvidar poner la acción por defecto en ACCEPT, o denegará todo el tráfico desde la red interna.</sup>
- Añadiríamos la regla en la IN u OUT y en el LOCAL.
- Despues realizar una prueba para comprobar la acción deseada.

<sup>En EdgeMax no es necesario añadir la misma regla en IN y OUT. Denegará o permitirá la acción deseada en cualquiera de ellas.</sup>


### Configurar una interfaz PPPoE de Movistar o O2 en un EdgeRouter de Ubiquiti
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/> Asegúrate de cambiar los parámetros del ISP y utilizar los que el ISP os indique.</p>


<p>1. Lo primero es entrar en la web de gestión del Edgerouter y pulsar en la pestaña Wizards de la parte superior derecha. Esto nos cargara un grupo de asistentes de configuración en la parte izquierda. Pulsamos sobre el que se llama WAN + +2LAN2. Esto nos cargara un formulario que deberemos rellenar con los datos de acuerdo a nuestras necesidades.</p>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/WAN/pppoe_1.png" alt="pppoe_1.png"></p>

<p>2. Internet port: En esta sección definiremos como está conectado nuestro Edgerouter al router HGU de Movistar o O2.</p>
<p><code>Port</code>: En el menú despegable seleccionamos el puerto de ethernet con el que está conectado al router HGU de Movistar o O2, etho o eth4.</p>
<p><code>Internet connection type</code>: Aquí seleccionamos PPPoE y rellenamos los campos de ls siguiente manera:</p>
<p><ul><code>Account name</code>: adsl@telefonicapa</ul></p>
<p><ul><code>Password</code>: adslppp</ul></p>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/WAN/pppoe_2.png" alt="pppoe_2.png"></p>

<p>3. LAN ports: Desplegando está sección podremos configurar la IP que tendrá nuestro router y habilitaremos el DHCP por defecto para que asigne IPs a aquellos equipos que se conecten al router.</p>
<sup>Tener en cuenta que el rango de IP debe ser distinto al que esta nuestro Edgerouter con el router HGU de Movistar o O2. La opción de DHCP viene habilitada por defecto, así que no la tocamos y la dejamos como está.</sup>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/WAN/pppoe_3.png" alt="pppoe_3.png"></p>

<p>4. User setup: Por último, es recomendable cambiar la contraseña del usuario ubnt que viene por defecto por otra más segura.</p>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/WAN/pppoe_4.png" alt="pppoe_4.png"></p>

<p>Para aplicar la configuración definida, pulsamos sobre Apply.</p>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/> No toméis estos pasos al pie de la letra. Utilízalos como una guía, ya que la configuración de vuestra red puede diferir con la de aquí expuesta. Pudiendo causar un mal funcionamiento de vuestra red.</p>

### LAN + DHCP
<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/> Asegúrate de cambiar el rando de la red a la de tu red y la interfaz a modificar

#### Modificar DHCP mediante CLI
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

#### Modificar DHCP mediante GUI
Lo primero es acceder a la web de gestión a la web de gestion del router. Una vez dentro en tramos en la pestaña <code>Services</code> y después en la sub-pestaña <code>DHCP Server</code>. Aquí se podrán ver los servicios <code>DHCP</code> que tenemos en marcha, si es la primera este listado estará vacío por lo que pulsamos en el botón <code>+ Add DHCP Server</code>.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/dhcp/dhcp_1.png" alt="dhcp_1.png"></p>

Nos aparecerá un formulario que deberemos rellenar con los datos adecuados a nuestras necesidades.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/dhcp/dhcp_2.png" alt="dhcp_2.png"></p>

<p>  &nbsp;<code>DHCP Name</code>: Podremos darle un nombre al servicio pero eso si, no se pueden utilizar espacios.
<p>  &nbsp;<code>Subnet</code>: Definimos la subred que ya tengamos configurada en alguna interfaz de nuestro router.
<p>  &nbsp;<code>Range Start</code>: metemos la dirección IP por la que empezara el rango que queremos que sirva nuestro DHCP.
<p>  &nbsp;<code>Range Stop</code>: la dirección IP fin del rango de direcciones a repartir.
<p>  &nbsp;<code>Router</code>: Este sería el Gateway, es decir, la salida a otras redes de nuestra LAN, ya sea internet o aotras redes.
<p>  &nbsp;<code>DNS 1</code>: Dirección IP del servidor DNS primario.
<p>  &nbsp;<code>DNS 2</code>:: Dirección IP del servidor DNS secundario.
<p>  &nbsp;<code>:Enable</code>:* Marcamos este checkbox para que una vez pulsemos el botón Save, se guarde la configuración y esta empiece a funcionar. Si no lo marcamos, la configuración se guardará pero esta no estará habilitada, así que el servicio no empezara a repartir direcciones IP.

Por ultimo pulsamos en el botón <code>Save</code>. Desde ese mismo momento cualquier dispositivo que se conecte a la red de nuestro router y solicite una dirección IP, el servicio que acabamos de configurar le asignara una del rango predefinido.

#### Ver estado del DHCP
Ahora que está en marcha podemos interactuar con el servicio pudiendo cambiar su configuración o viendo el estado de asignaciones <code>(leases)</code> de direcciones IP.

Para ello en basta con pulsar en el menú desplegable <code>Actions</code> y después en <code>Viewe Details</code>.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/dhcp/dhcp_3.png" alt="dhcp_3.png"></p>

Se nos cargara las características del servicio pudiendo cambiarlas si es que lo deseamos. También aparecerá un resumen del estado del servicio, como la cantidad de IPs tiene de para repartir, cuantas estas asignadas, cuantas dispone para repartir etc.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/dhcp/dhcp_4.png" alt="dhcp_4.png"></p>

También hay opción de asignar una dirección del rango de manera estática a un dispositivo de nuestra red. Bastara con pulsar en <code>Create New Mapping</code> y asignar un IP del rango a la dirección MAC del dispositivo.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/dhcp/dhcp_5.png" alt="dhcp_5.png"></p>

En la pestaña <code>Leases</code> nos encontraremos con aquellas direcciones que ya están asignadas a algún dispositivo. Pudiendo ver cuánto tiempo les queda de asignación y pudiendo asignar de manera estática la IP que ya tienen asignada.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/dhcp/dhcp_6.png" alt="dhcp_6.png"></p>


### Port Forwarding
Seleccione las interfaces WAN y LAN que se utilizarán para el reenvío de puertos.
- Pueden realizar el procedimiento mediante la GUI o mediante CLI.
- Mediante CLI: Firewall/NAT > Port Forwarding

<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/>  Asegúrate de cambiar el rando de la red a la de tu red y la interfaz a modificar
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

### DNS DINAMICO

<details>
    <summary>Mediante interfaz GUI:</summary>

## Instrucciones de uso con GUI:
<sup><strong><font style="vertical-align: inherit;">ATENCIÓN: </font></strong> Para poder obtener el token y el dominio DuckDNS pueden obtenerlo desde este repositorio <a href="https://github.com/JuanRodenas/Duckdns">DuckDNS</a>.</sup>

1. Estando dentro de la web de gestión entramos en la pestaña <code>Service</code> y a continuación en <code>DNS</code>. Por ultimo en la sección <em>Dynamic DNS</em> pulsamos el botón <code>+ Add DDNS Interface</code>.
2. Se cargará un formulario vació que deberemos rellenar con los datos adecuados:
<ul><code>Interface: Aquí hay que seleccionar la interfaz en la que está configurada nuestra IP pública.</code></ul>
<ul><code>Service: En el menú desplegable hay varios servicios ya pre-configurados, pero entre ellos al no estar DuckDNS optamos por la opción custom.</code></ul>
<ul><code>Hostname: Aquí hay que meter el subdominio DuckDNS que queremos asignar a nuestro router. Solamente el subdominio, no hace falta meter .duckdns.org.</code></ul>
<ul><code>Login: poniendo nouser servirá, ya que nos identificaremos mediante nuestro Token.</code></ul>
<ul><code>Password: Aquí deberemos introducir el Token de nuestra cuenta.</code></ul>
<ul><code>Protocol: Seleccionamos el protocolo dyndns2.</code></ul>
<ul><code>Server: por último metemos la url del servidor de DuckDNS, www.duckdns.org.</code></ul>
3. Para terminar pulsamos en Apply para guardar todo lo que hemos metido.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/DuckDNS/gui1.png" alt="GUI1"></p>
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/DuckDNS/gui2.png" alt="GUI2"></p>
&nbsp;
</details>
&nbsp;

<details>
    <summary>Mediante interfaz CLI:</summary>

## Instrucciones de uso con CLI:

Esto podemos realizarlo conectando al router mediante el protocolo SSH o usando el intérprete CLI incorporado en la propia web de gestión.
En todo caso ya sea mediante un método u otro, deberemos iniciar sesión utilizando las mismas credenciales que usamos para acceder vía web.

1. Accedemos por ssh o cli web.
2. 
<img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/atencion.png" alt="atencion" width="20"/>  <sup><strong><font style="vertical-align: inherit;">ATENCIÓN: </font></strong> Teneis que cambiar el "SUBDMIONIO" y "TOKEN".</sup>
```sh
configure
set service dns dynamic interface INTERFAZ service custom-duckdns
set service dns dynamic interface INTERFAZ service custom-duckdns host-name SUBDMIONIO
set service dns dynamic interface INTERFAZ service custom-duckdns login nouser
set service dns dynamic interface INTERFAZ service custom-duckdns password TOKEN
set service dns dynamic interface INTERFAZ service custom-duckdns protocol dyndns2
set service dns dynamic interface INTERFAZ service custom-duckdns server www.duckdns.org
commit ; save
```
3. Periódicamente el router ira actualizando nuestra IP pública en el DNS de DuckDNS. En caso de querer forzar la actualización, se puede realizar lanzando el siguiente comando.
<ul><code>update dns dynamic interface INTERFAZ</code></ul>
4. Si hemos realizado bien todos los pasos anteriores, ejecutando el siguiente comando veremos si todo está funcionando como debería.
show dns dynamic status
<ul><code>
interface    : INTERFAZ
ip address   : xxx.xxx.xxx.xxx
host-name    : SUBDOMINIO
last update  : Tue Sep 29 22:28:09 2020
update-status: good
</code></ul>
<p>Si en el apartado <code>update-status:</code> vemos que aparece <code>good</code> es que todo está funcionando perfectamente.</p>

&nbsp;
</details>
&nbsp;

---
## <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/block.png" alt="Ubiquiti edgemax" width="40"/> Añadir listas de seguridad al firewall

#### Creamos el grupo y agregamos una regla de firewall a la WAN:
* Creamos un nuevo grupo y modificamos nombre de grupo.
<p><sup>Para ver los grupos que tenemos: <code>show firewall group network-group</code>.</sup></p>

```
set firewall group network-group SPAMHAUS_DROP
commit
```

* Para añadir la regla en el firewall, modificamos el número de regla y cambiamos el <code>network-group</code> con el nombre del grupo creado. 
<p><sup>Para ver la regla y el orden: <code>show firewall name WAN_IN</code>.</sup></p>

```
set firewall name WAN_IN rule 10 source group network-group SPAMHAUS_DROP
set firewall name WAN_IN rule 10 description "networks to drop from spamhaus.org list"
set firewall name WAN_IN rule 10 action drop
set firewall name WAN_IN rule 10 state established enable
set firewall name WAN_IN rule 10 state related enable
set firewall name WAN_IN rule 10 protocol all
commit ; save
```

#### Crear y Añadir el script /config/scripts/post-config.d/update-spamhaus
<p>Modificamos en el script el nombre de los argumentos: <code>NETGROUP</code>, <code>TMPFILE</code> y <code>TMPFILE2</code> con el nombre del grupo creado.</p>
<p>Las listas a añadir tienen que tener formato <code>.raw</code> o <code>.txt</code>.</p>
<p>EDIT: Crear el script en <code>/config/scripts/post-config.d</code> mejor que en <code>/config/scripts/</code> porque después de un reinicio el grupo de firewall volverá a estar vacío, pero si el script está en ese directorio <code>/config/scripts/post-config.d</code>, se ejecutará automáticamente después del arranque.</p>

```sh
sudo vi /config/scripts/post-config.d/update-spamhaus
```

<p>El scrip pueden descargarlo o verlo tambien desde el repositorio en este: <a href="https://github.com/JuanRodenas/Ubiquiti/blob/main/SPAMHAUS_DROP">link</a></p>

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
<p>El comando VI del equipo no está completo, por lo que para guardar, utilizar ZZ o :wq</p>

#### Hazlo ejecutable:
```
sudo chmod +x /config/scripts/post-config.d/update-spamhaus
```

EJECUTAR:
```
sudo /config/scripts/post-config.d/update-spamhaus
```
<p><sup>No pensar que se ha quedado bloqueado al insertar el comando, tarda un poco si la lista es muy grande.</sup></p>

Resultado:
<ul><code>Added 561 entries to SPAMHAUS_DROP</code></ul>

  
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



#### Buenas prácticas.
<p>Buenas prácticas para un correcto funcionamiento del firewall:</p>

- Coloque la regla spamhaus en primer lugar en WAN_IN y WAN_LOCAL (es decir, antes de la regla de permiso para conexiones establecidas y relacionadas). Esto es para evitar la situación "rara" de que un host interno (por ejemplo, infectado con malware) de alguna manera establezca una conexión con un host listado de spamhaus, dando la oportunidad de usar la conexión establecida para fines de spam.
- Ponga la regla de spamhaus en WAN_OUT, otra vez antes que cualquier otra cosa.
- Hoy noté en mis registros que el WAN_OUT coincidió (y rechazó) con el tráfico saliente a la dirección IP 185.3.135.146 (búsqueda de spamhaus aquí, listado desde el 29/2/2016). Este tráfico se originó en el cliente bittorrent que se ejecuta en mi NAS. No sé si los spammers usan bittorrent para infiltrarse en hosts posiblemente vulnerables, pero lo considero como un paso de protección adicional que funcionó.
- Debería asignar las reglas de firewall solo en el pppoe 


### REVISIÓN
<p>Listar</p>
<ul><code>sudo /sbin/ipset list</code></ul>
<p>Utilice este comando a través de la CLI para ver las entradas:</p>
<ul><code>show firewall group SPAMHAUS_DROP</code></ul>
<p>Despues vemos las tareas</p>
<ul><code>show system task-scheduler</code></ul>
<p>Ver log</p>
<ul><code>cat /var/log/messages</code></ul>

* He creado un script para poder informaros al telegram de las IPs bloqueadas por una regla.
<p>El scrip pueden descargarlo o verlo desde el repositorio en este: <a href="https://github.com/JuanRodenas/Ubiquiti/blob/main/honeypot">link</a></p>


### EJEMPLO DE REGLAS:
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
### EJEMPLO DE INTERFAZ WAN:
```sh
ethernet eth1 {
        description "Internet (PPPoE)"
        duplex auto
        firewall {
            in {
                name WAN_IN
            }
            local {
                name WAN_LOCAL
            }
        }
        pppoe 0 {
            default-route auto
            firewall {
                in {
                    name WAN_IN
                }
                local {
                    name WAN_LOCAL
                }
            }
```

---
## <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/icon-certificate.png" alt="Ubiquiti edgemax" width="40"/> POSIBILIDAD DE AÑADIR UN CERTIFICADO A LOCALHOST
 

### Añadir certificado CA para localhost
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
## Configuración de copia de seguridad y restauración 
Realizar una copia de seguridad y restaurar el archivo de configuración de un EdgeRouter.

<details>
    <summary>Realización copia de seguridad y restauración vía GUI:</summary>

## Instrucciones de uso para realizar vía GUI

1. Navegue al sistema en la parte inferior izquierda de la GUI para descargar el archivo de configuración de la copia de seguridad.
<ul><code>**Sistema** > **Gestión de configuración** y **mantenimiento de dispositivos** > **Back Up Config**</code></ul>
2. Descargue el archivo de configuración de la copia de seguridad haciendo clic en el Descargar .
3. EdgeRouter le pedirá que guarde el archivo en su ordenador.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/Backup-restore/backup.PNG" alt="backup"></p>

## Instrucciones de uso para restaurar vía GUI
1. Navegue al sistema en la parte inferior izquierda de la GUI para descargar el archivo de configuración de la copia de seguridad.
<ul><code>**Sistema** > **Gestión de configuración** y **mantenimiento de dispositivos** > **Restore Config**</code></ul>
2. Cargue el archivo de configuración de la copia de seguridad haciendo clic en el **Upload a file** .
3. EdgeRouter solicitará que se reinicie el dispositivo para completar la restauración.
<p><img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/Backup-restore/restore.PNG" alt="restore"></p>

## Instrucciones de uso para realizar/restaurar vía UNMS
Para realizar o restaurar vía UNMS deben seguir los pasos de este artículo:
<ul><a href="https://help.ui.com/hc/en-us/articles/360002535514">realizar o restaurar vía UNMS</a></ul>

&nbsp;
</details>
&nbsp;

<details>
    <summary>Realización copia de seguridad y restauración vía CLI:</summary>

## Instrucciones de uso para realizar vía CLI

<p>1. Puede hacerlo usando el botón CLI en la GUI o usando un programa como PuTTY.</p>
<p>2. Ingrese al modo de configuración y asegúrese de que todos los cambios en las configuraciones actualmente activas/en funcionamiento se guarden en la arranque/inicio.</p>
<ul><code>commit ; save</code></ul>
<p>3. Guarde el archivo de configuración <code>config.boot</code> en una máquina remota mediante una de estas opciones: TFTP, SCP, FTP o SFTP.</p> 

```sh
  scp://<user>:<passwd>@<host>/<file>   Save to file on remote machine
  sftp://<user>:<passwd>@<host>/<file>  Save to file on remote machine
  ftp://<user>:<passwd>@<host>/<file>   Save to file on remote machine
  tftp://<host>/<file>                  Save to file on remote machine
```
Y con el comando <code>**save tftp://host/config.boot**</code> guardamos el archivo de configuración.
<p>4. Verifique el contenido de la configuración de inicio abriendo el <code>config.boot</code> con un editor de texto y compare con el del equipo que se haya exportado correctamente.</p>
<ul><code>cat /config/config.boot</code></ul>

## Instrucciones de uso para restaurar vía CLI
<p>1. Puede hacerlo usando el botón CLI en la GUI o usando un programa como PuTTY.</p> 
<p>2. Compare las diferencias entre la respaldo/funcionamiento y la activa.</p>
<p>3. Guarde el archivo de configuración <code>config.boot</code> en una máquina remota mediante una de estas opciones: TFTP, SCP, FTP o SFTP.</p>

```sh
  scp://<user>:<passwd>@<host>/<file>   Load from file on remote machine
  sftp://<user>:<passwd>@<host>/<file>  Load from file on remote machine
  ftp://<user>:<passwd>@<host>/<file>   Load from file on remote machine
  http://<host>/<file>                  Load from file on remote machine
  tftp://<host>/<file>                  Load from file on remote machine
```

Y con el comando <code>**load tftp://host/config.boot**</code> guardamos el archivo de configuración.
<p>4. Verifique que la restauración ha sido correcta y con el contenido de la configuración del <code>config.boot</code> con un editor de texto y compare con el del equipo que se haya importado correctamente.</p>
<ul><code>cat /config/config.boot</code> y con el comando <code>compare</code></ul>
<p>5. Una vez asegurado de que todos los cambios en las configuraciones actualmente activas/en funcionamiento son correctas se procede a guardar en el arranque/inicio.
<ul><code>commit ; save</code></ul></p>

&nbsp;
</details>
&nbsp;

- También hay una opción que nos indican Ubiquiti, ellos la llaman **desinfectar** o **limpiar** las configuraciones de EdgeRouter para eliminar toda la información personal y confidencial.
Ubiquiti nos dedica un articulo muy detallado para esta opción. Esta opción de **desinfectar** es cuando necesitas ayuda y quieres enviar la plantilla o "cachos" de la plantilla al foro o fabricante.
<ul><a href="https://help.ui.com/hc/en-us/articles/360012074414">Desinfectar las configuraciones de EdgeRouter</a></ul>

## Conclusión
Con esta información puedes configurar un router neutro que realice solamente las funciones de un router, este dispositivo puede ser una buena opción. Aquí podrás encontrar todo lo que he conseguido hacer con este router.
<p><sup>Iré actualizando información y añadiendo procedimientos cuando tenga tiempo libre.</sup></p>

<blockquote class="is-info"><p>Los pasos anteriormente explicados están basados en una red que puede diferir de la que tú tienes montada. Si sigues al pie de la letra todos los pasos, pueden no coincidir con la configuración de tu <em>red</em> y dejarla inservible. Adapta en todo momento la documentación que se ha expuesto para que cuadre con tu red.</p></blockquote>
