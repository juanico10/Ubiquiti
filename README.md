# Wiki Ubiquiti
A collection of enhancements for EdgeMax based devices

## EdgeRouter 4 initial setup
### Setup user

#### Login to router and add a new user

```sh
$ ssh ubnt@<ip-of-edgerouter>
configure
set system login user <user> authentication plaintext-password <secret>
set system login user <user> level admin
commit
save
exit
logout
```

#### Remove default user

```sh
$ ssh ubnt@<ip-of-edgerouter>
configure
delete system login user ubnt
commit  
save  
exit
logout
```

#### Add a public ssh key to EdgeRouter

```sh
$ scp ~/.ssh/id_rsa.pub <ip-of-edgerouter>:/tmp
```

```sh
$ ssh <user>@<ip-of-edgerouter>
configure  
loadkey <user> /tmp/id_rsa.pub  
commit  
save  
exit
sudo chown -R <user> /home/<user>
logout
```

#### Sanity check

```sh
$ ssh <user>@<ip-of-edgerouter>
exit
```

#### Disable plain text password authentication
If you can successfully login to EdgeRouter, a step to hardening security on your EdgeRouter is to remove option to use plain text password.  
**NOTE!** Make sure you can access with your public key before disabling plaintext authentication.

```sh
$ ssh <user>@<ip-of-edgerouter>
configure
set service ssh disable-password-authentication
commit; save
exit
logout
```

### Setup Edgerouter 4

#### Add CA certificate for localhost

Using [mkcert](https://words.filippo.io/mkcert-valid-https-certificates-for-localhost/) by [Filippo Valsorda](https://filippo.io/) to create a CA cert for localhost.  
Option to go the SSL cert route with Let´s Encrypt there is several diffrent to choose from. Ex. [ubnt-letsencrypt](https://github.com/j-c-m/ubnt-letsencrypt) by [Jesse Miller](https://github.com/j-c-m)

```sh
$ ssh <user>@<ip-of-edgerouter>
configure
set system static-host-mapping host-name <hostname> inet <ip-of-edgerouter>
commit
save
exit
```

**Create certificate**

```sh
$ mkcert <ip-of-edgerouter> <hostname>
cat <ip-of-edgerouter>+1-key.pem <ip-of-edgerouter>+1.pem > server.pem
```

**Backup existing certificate file**

```sh
$ ssh <user>@<ip-of-edgerouter>
sudo cp /etc/lighttpd/server.pem /etc/lighttpd/.server-OLD.pem
exit
```

**Copy new certificate file to user home dir of your router**

```sh
scp /path/to/server.pem <user>@<ip-of-edgerouter>:/home/<user>/server.pem
```

**Copy new certificate file from user home dir and enable the certificate**

```sh
$ ssh <user>@<ip-of-edgerouter>
sudo cp /home/<user>/server.pem /etc/lighttpd/server.pem
# Kill webserver service by PID
sudo kill -SIGINT $(cat /var/run/lighttpd.pid)
# Start webserver
sudo /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
exit
```

**Check your connection with curl**  
If done correctly, one way of checking is to use curl. If you get a redirect to a SSL protocol port, i.e 443, the certificate is installed correctly in your router.

```sh
$ curl -I http:/<ip-of-edgerouter>
HTTP/1.1 301 Moved Permanently
Location: https://<ip-of-edgerouter>:443/
Date: Sun, 11 Jan 2015 07:46:13 GMT
Server: Server
```

#### Firewall

```sh
$ ssh <user>@<ip-of-edgerouter>
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
commit
save
exit
```

#### WAN

```sh
$ ssh <user>@<ip-of-edgerouter>
configure
set interfaces ethernet eth0 description WAN
set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth0 firewall in name WAN_IN
set interfaces ethernet eth0 firewall local name WAN_LOCAL
set service nat rule 5010 description "Masquerade for WAN"
set service nat rule 5010 outbound-interface eth0
set service nat rule 5010 type masquerade
commit
save
exit
```

#### LAN

```sh
$ ssh <user>@<ip-of-edgerouter>
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
commit
save
exit
```
