# Examen 1ª Evaluación - Ramón Manuel Porto Hombre

## 1.Explica métodos para 'abrir' una consola/shell a un contenedor que se está ejecutando

Con el comando docker exec podemos abrir una terminal al abrir el contenedor su sintaxis es la siguiente:
~~~
docker exec contenedor bash
~~~ 
contenedor sería el nombre del contenedor y bash es el intérprete.

También existe la opción de una vez iniciado el contenedor, se haga clic derecho sobre él y presionar la opción "*Attach shell*"

## 2.En el contenedor anterior con que opciones tiene que haber sido arrancado para poder interactuar con las entradas y salidas del contenedor

Es el mismo comando anterior añadiendo la opción "***-it***", que es la que permite interactuar con el contenedor.

~~~
docker exec -it contenedor bash
~~~ 
    
## 3.¿Cómo sería un fichero docker-compose para que dos contenedores se comuniquen entre si en una red solo de ellos?
Habría que crear la subred en el docker-compose.yml de la siguente forma:
~~~
networks:
    my-network:
      ipam:
        config:
          - subnet: 10.0.9.0/24
            gateway: 10.0.9.0
~~~

Posteriormente añadiríamos el apartado networks a cada contenedor de la siguiente forma:
~~~
networks:
      my-network:
        ipv4_address: 10.0.9.33
~~~

Ambos contenedores deben estar en la misma red para poder comunicarse entre si.    

## 4.¿Qué hay que añadir al fichero anterior para que un contenedor tenga la IP fija?

Se debe añadir la opción "**ipv4_address**" y a continuación la IP deseada para asignarsela de manera fija.
~~~
networks:
      my-network:
        ipv4_address: 10.0.9.33
~~~
En este caso la ip asignada sería la 10.0.9.33    
## 5.¿Que comando de consola puedo usar para saber las ips de los contenedores anteriores? Filtra todo lo que puedas la salida.

Mediante el comando docker inspect podemos obtener las IPs de los contenedores. La sintaxis es la siguiente:

~~~
docker inspect bin9_subnet
~~~

Inspeccionamos la red a la que pertenecen los contenedores, *bind9_subnet* en este caso y obtenemos la siguiente salida por consola.

Salida por consola del comando:
```
console
"4e86cf951c0444c4af33e712531bea5cdf46333259b9c6a049a79232326fb552": {
                "Name": "asir_bind9",
                "EndpointID": "09e81eba45a3b67f2d665aebb18053c1991e9f45b30dadcc65ad1426eb61662e",
                "MacAddress": "02:42:ac:1c:05:01",
                "IPv4Address": "172.28.5.1/24",
                "IPv6Address": ""
            },
            "f32cba4c774aaa37087290486cdadee492230b42c6ebe12d068803bf61c5a0aa": {
                "Name": "asir_cliente_dns",
                "EndpointID": "77d78f17cf027f9306b2a763104d8e9dfb1041b37027440cddf77a3db6fd1205",
                "MacAddress": "02:42:ac:1c:05:21",
                "IPv4Address": "172.28.5.33/24",
                "IPv6Address": ""
            }
```


## 6.¿Cual es la funcionalidad del apartado "ports" en docker compose?
Sirve para mapear los puertos del contenedor con los puertos del host.
Un ejemplo:
~~~
ports:
- "53:53"
~~~
En este caso se mapea el puerto 53 tanto del contenedor como del host.   
   

## 7.¿Para que sirve el registro CNAME? Pon un ejemplo
Indica que un dominio es un alias de otro dominio.

mi-sitio.com    IN  A   33.33.33.33
www     CNAME   mi-sitio.com

En este ejemplo al consultar www.mi-sitio.com devolvería mi-sitio.com

## 8.¿Como puedo hacer para que la configuración de un contenedor DNS no se borre si creo otro contenedor?

Mediante el uso de volúmenes. El comando es el siguiente:

~~~
docker volume create volumen
~~~
Siendo volumen el nombre que le queramos poner al volumen.
   
## 9.Añade una zona tiendadeelectronica.int en tu docker DNS que tenga

En primer lugar, una vez se hayan configurado correctamente los ficheros de configuración y el docker compose, procedemos a lanzar dicho archivo con el siguiente comando:

~~~
docker compose up -d
~~~
La opción -d es para que se ejecute en segundo plano.

Una vez tenemos los contenedores iniciados haremos un attach shell al contenedor tal y como se muestra en la siguiente imagen:

![ALT](/home/asir2/Documentos/SRI/Git/Examen/2.png)

Ahora comprobaremos que funciona correctamente mediante el comando dig, en primer lugar comprobamos *www.tiendadeelectronica.int*
![ALT](/home/asir2/Documentos/SRI/Git/Examen/3.png)

Seguimos comprobando el registro *CNAME*
![ALT](/home/asir2/Documentos/SRI/Git/Examen/4.png)

Por último comprobamos el registro *TXT*

![ALT](/home/asir2/Documentos/SRI/Git/Examen/5.png)

Para finalizar vemos los logs haciendo clic derecho sobre el contenedor y seleccionando la opción "*View Logs*"

!(/home/asir2/Documentos/SRI/Git/Examen/6.png)

Salida por consola de los logs:
```
console
Starting named...
exec /usr/sbin/named -u "bind" "-g" ""
13-Nov-2023 15:27:21.153 starting BIND 9.18.12-1ubuntu1.1-Ubuntu (Extended Support Version) <id:>
13-Nov-2023 15:27:21.153 running on Linux x86_64 6.1.0-13-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.55-1 (2023-09-29)
13-Nov-2023 15:27:21.153 built with  '--build=x86_64-linux-gnu' '--prefix=/usr' '--includedir=${prefix}/include' '--mandir=${prefix}/share/man' '--infodir=${prefix}/share/info' '--sysconfdir=/etc' '--localstatedir=/var' '--disable-option-checking' '--disable-silent-rules' '--libdir=${prefix}/lib/x86_64-linux-gnu' '--runstatedir=/run' '--disable-maintainer-mode' '--disable-dependency-tracking' '--libdir=/usr/lib/x86_64-linux-gnu' '--sysconfdir=/etc/bind' '--with-python=python3' '--localstatedir=/' '--enable-threads' '--enable-largefile' '--with-libtool' '--enable-shared' '--disable-static' '--with-gost=no' '--with-openssl=/usr' '--with-gssapi=yes' '--with-libidn2' '--with-json-c' '--with-lmdb=/usr' '--with-gnu-ld' '--with-maxminddb' '--with-atf=no' '--enable-ipv6' '--enable-rrl' '--enable-filter-aaaa' '--disable-native-pkcs11' 'build_alias=x86_64-linux-gnu' 'CFLAGS=-g -O2 -ffile-prefix-map=/build/bind9-2zwQl8/bind9-9.18.12=. -flto=auto -ffat-lto-objects -fstack-protector-strong -Wformat -Werror=format-security -fdebug-prefix-map=/build/bind9-2zwQl8/bind9-9.18.12=/usr/src/bind9-1:9.18.12-1ubuntu1.1 -fno-strict-aliasing -fno-delete-null-pointer-checks -DNO_VERSION_DATE -DDIG_SIGCHASE' 'LDFLAGS=-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now' 'CPPFLAGS=-Wdate-time -D_FORTIFY_SOURCE=2'
13-Nov-2023 15:27:21.153 running as: named -u bind -g
13-Nov-2023 15:27:21.153 compiled by GCC 12.2.0
13-Nov-2023 15:27:21.153 compiled with OpenSSL version: OpenSSL 3.0.8 7 Feb 2023
13-Nov-2023 15:27:21.153 linked to OpenSSL version: OpenSSL 3.0.8 7 Feb 2023
13-Nov-2023 15:27:21.153 compiled with libxml2 version: 2.9.14
13-Nov-2023 15:27:21.153 linked to libxml2 version: 20914
13-Nov-2023 15:27:21.153 compiled with json-c version: 0.16
13-Nov-2023 15:27:21.153 linked to json-c version: 0.16
13-Nov-2023 15:27:21.153 compiled with zlib version: 1.2.13
13-Nov-2023 15:27:21.153 linked to zlib version: 1.2.13
13-Nov-2023 15:27:21.153 ----------------------------------------------------
13-Nov-2023 15:27:21.153 BIND 9 is maintained by Internet Systems Consortium,
13-Nov-2023 15:27:21.153 Inc. (ISC), a non-profit 501(c)(3) public-benefit 
13-Nov-2023 15:27:21.153 corporation.  Support and training for BIND 9 are 
13-Nov-2023 15:27:21.153 available at https://www.isc.org/support
13-Nov-2023 15:27:21.153 ----------------------------------------------------
13-Nov-2023 15:27:21.153 found 12 CPUs, using 12 worker threads
13-Nov-2023 15:27:21.153 using 12 UDP listeners per interface
13-Nov-2023 15:27:21.153 DNSSEC algorithms: RSASHA1 NSEC3RSASHA1 RSASHA256 RSASHA512 ECDSAP256SHA256 ECDSAP384SHA384 ED25519 ED448
13-Nov-2023 15:27:21.153 DS algorithms: SHA-1 SHA-256 SHA-384
13-Nov-2023 15:27:21.153 HMAC algorithms: HMAC-MD5 HMAC-SHA1 HMAC-SHA224 HMAC-SHA256 HMAC-SHA384 HMAC-SHA512
13-Nov-2023 15:27:21.153 TKEY mode 2 support (Diffie-Hellman): yes
13-Nov-2023 15:27:21.153 TKEY mode 3 support (GSS-API): yes
13-Nov-2023 15:27:21.157 config.c: option 'trust-anchor-telemetry' is experimental and subject to change in the future
13-Nov-2023 15:27:21.157 loading configuration from '/etc/bind/named.conf'
13-Nov-2023 15:27:21.157 unable to open '/etc/bind/bind.keys'; using built-in keys instead
13-Nov-2023 15:27:21.157 looking for GeoIP2 databases in '/usr/share/GeoIP'
13-Nov-2023 15:27:21.157 using default UDP/IPv4 port range: [32768, 60999]
13-Nov-2023 15:27:21.157 using default UDP/IPv6 port range: [32768, 60999]
13-Nov-2023 15:27:21.157 listening on IPv4 interface lo, 127.0.0.1#53
13-Nov-2023 15:27:21.157 listening on IPv4 interface eth0, 172.28.5.1#53
13-Nov-2023 15:27:21.157 generating session key for dynamic DNS
13-Nov-2023 15:27:21.157 sizing zone task pool based on 6 zones
13-Nov-2023 15:27:21.157 none:99: 'max-cache-size 90%' - setting to 14187MB (out of 15763MB)
13-Nov-2023 15:27:21.157 using built-in root key for view _default
13-Nov-2023 15:27:21.157 set up managed keys zone for view _default, file 'managed-keys.bind'
13-Nov-2023 15:27:21.157 configuring command channel from '/etc/bind/rndc.key'
13-Nov-2023 15:27:21.157 command channel listening on 127.0.0.1#953
13-Nov-2023 15:27:21.157 configuring command channel from '/etc/bind/rndc.key'
13-Nov-2023 15:27:21.161 command channel listening on ::1#953
13-Nov-2023 15:27:21.161 not using config file logging statement for logging due to -g option
13-Nov-2023 15:27:21.161 managed-keys-zone: loaded serial 0
13-Nov-2023 15:27:21.161 zone 0.in-addr.arpa/IN: loading from master file /etc/bind/db.0 failed: file not found
13-Nov-2023 15:27:21.161 zone 0.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:27:21.161 zone 127.in-addr.arpa/IN: loading from master file /etc/bind/db.127 failed: file not found
13-Nov-2023 15:27:21.161 zone 127.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:27:21.161 zone 255.in-addr.arpa/IN: loading from master file /etc/bind/db.255 failed: file not found
13-Nov-2023 15:27:21.161 zone 255.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:27:21.161 zone localhost/IN: loading from master file /etc/bind/db.local failed: file not found
13-Nov-2023 15:27:21.161 zone localhost/IN: not loaded due to errors.
13-Nov-2023 15:27:21.161 /var/lib/bind/db.tiendadeelectronica.int:13: file does not end with newline
13-Nov-2023 15:27:21.161 zone tiendadeelectronica.int/IN: NS 'db.tiendadeelectronica.int' has no address records (A or AAAA)
13-Nov-2023 15:27:21.161 zone tiendadeelectronica.int/IN: not loaded due to errors.
13-Nov-2023 15:27:21.161 all zones loaded
13-Nov-2023 15:27:21.161 running
13-Nov-2023 15:27:21.165 managed-keys-zone: No DNSKEY RRSIGs found for '.': success
13-Nov-2023 15:28:20.556 no valid RRSIG resolving 'com/DS/IN': 1.1.1.1#53
13-Nov-2023 15:28:20.564 no valid RRSIG resolving 'com/DS/IN': 8.8.8.8#53
13-Nov-2023 15:28:20.564 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:28:20.564 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:28:21.576 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:28:21.576 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:28:21.580 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:28:21.580 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:28:23.588 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:28:23.588 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:28:23.592 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:28:23.592 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:32:14.947 validating ./NS: got insecure response; parent indicates it should be secure
13-Nov-2023 15:32:14.947 insecurity proof failed resolving './NS/IN': 8.8.8.8#53
13-Nov-2023 15:32:14.959 validating ./NS: got insecure response; parent indicates it should be secure
13-Nov-2023 15:32:14.959 insecurity proof failed resolving './NS/IN': 1.1.1.1#53
13-Nov-2023 15:34:39.949 no longer listening on 127.0.0.1#53
13-Nov-2023 15:34:39.949 no longer listening on 172.28.5.1#53
13-Nov-2023 15:34:39.953 shutting down
13-Nov-2023 15:34:39.953 stopping command channel on 127.0.0.1#953
13-Nov-2023 15:34:39.953 stopping command channel on ::1#953
13-Nov-2023 15:34:39.953 automatic interface scanning terminated: end of file
13-Nov-2023 15:34:39.961 exiting
Starting named...
exec /usr/sbin/named -u "bind" "-g" ""
13-Nov-2023 15:34:40.761 starting BIND 9.18.18-0ubuntu0.23.04.1-Ubuntu (Extended Support Version) <id:>
13-Nov-2023 15:34:40.761 running on Linux x86_64 6.1.0-13-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.55-1 (2023-09-29)
13-Nov-2023 15:34:40.761 built with  '--build=x86_64-linux-gnu' '--prefix=/usr' '--includedir=${prefix}/include' '--mandir=${prefix}/share/man' '--infodir=${prefix}/share/info' '--sysconfdir=/etc' '--localstatedir=/var' '--disable-option-checking' '--disable-silent-rules' '--libdir=${prefix}/lib/x86_64-linux-gnu' '--runstatedir=/run' '--disable-maintainer-mode' '--disable-dependency-tracking' '--libdir=/usr/lib/x86_64-linux-gnu' '--sysconfdir=/etc/bind' '--with-python=python3' '--localstatedir=/' '--enable-threads' '--enable-largefile' '--with-libtool' '--enable-shared' '--disable-static' '--with-gost=no' '--with-openssl=/usr' '--with-gssapi=yes' '--with-libidn2' '--with-json-c' '--with-lmdb=/usr' '--with-gnu-ld' '--with-maxminddb' '--with-atf=no' '--enable-ipv6' '--enable-rrl' '--enable-filter-aaaa' '--disable-native-pkcs11' 'build_alias=x86_64-linux-gnu' 'CFLAGS=-g -O2 -ffile-prefix-map=/build/bind9-VMBSpu/bind9-9.18.18=. -flto=auto -ffat-lto-objects -fstack-protector-strong -Wformat -Werror=format-security -fdebug-prefix-map=/build/bind9-VMBSpu/bind9-9.18.18=/usr/src/bind9-1:9.18.18-0ubuntu0.23.04.1 -fno-strict-aliasing -fno-delete-null-pointer-checks -DNO_VERSION_DATE -DDIG_SIGCHASE' 'LDFLAGS=-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now' 'CPPFLAGS=-Wdate-time -D_FORTIFY_SOURCE=2'
13-Nov-2023 15:34:40.761 running as: named -u bind -g
13-Nov-2023 15:34:40.761 compiled by GCC 12.3.0
13-Nov-2023 15:34:40.761 compiled with OpenSSL version: OpenSSL 3.0.8 7 Feb 2023
13-Nov-2023 15:34:40.761 linked to OpenSSL version: OpenSSL 3.0.8 7 Feb 2023
13-Nov-2023 15:34:40.761 compiled with libuv version: 1.44.2
13-Nov-2023 15:34:40.761 linked to libuv version: 1.44.2
13-Nov-2023 15:34:40.761 compiled with libxml2 version: 2.9.14
13-Nov-2023 15:34:40.761 linked to libxml2 version: 20914
13-Nov-2023 15:34:40.761 compiled with json-c version: 0.16
13-Nov-2023 15:34:40.761 linked to json-c version: 0.16
13-Nov-2023 15:34:40.761 compiled with zlib version: 1.2.13
13-Nov-2023 15:34:40.761 linked to zlib version: 1.2.13
13-Nov-2023 15:34:40.761 ----------------------------------------------------
13-Nov-2023 15:34:40.761 BIND 9 is maintained by Internet Systems Consortium,
13-Nov-2023 15:34:40.761 Inc. (ISC), a non-profit 501(c)(3) public-benefit 
13-Nov-2023 15:34:40.761 corporation.  Support and training for BIND 9 are 
13-Nov-2023 15:34:40.761 available at https://www.isc.org/support
13-Nov-2023 15:34:40.761 ----------------------------------------------------
13-Nov-2023 15:34:40.761 found 12 CPUs, using 12 worker threads
13-Nov-2023 15:34:40.761 using 12 UDP listeners per interface
13-Nov-2023 15:34:40.765 DNSSEC algorithms: RSASHA1 NSEC3RSASHA1 RSASHA256 RSASHA512 ECDSAP256SHA256 ECDSAP384SHA384 ED25519 ED448
13-Nov-2023 15:34:40.765 DS algorithms: SHA-1 SHA-256 SHA-384
13-Nov-2023 15:34:40.765 HMAC algorithms: HMAC-MD5 HMAC-SHA1 HMAC-SHA224 HMAC-SHA256 HMAC-SHA384 HMAC-SHA512
13-Nov-2023 15:34:40.765 TKEY mode 2 support (Diffie-Hellman): yes
13-Nov-2023 15:34:40.765 TKEY mode 3 support (GSS-API): yes
13-Nov-2023 15:34:40.765 config.c: option 'trust-anchor-telemetry' is experimental and subject to change in the future
13-Nov-2023 15:34:40.765 loading configuration from '/etc/bind/named.conf'
13-Nov-2023 15:34:40.765 unable to open '/etc/bind/bind.keys'; using built-in keys instead
13-Nov-2023 15:34:40.765 looking for GeoIP2 databases in '/usr/share/GeoIP'
13-Nov-2023 15:34:40.765 using default UDP/IPv4 port range: [32768, 60999]
13-Nov-2023 15:34:40.765 using default UDP/IPv6 port range: [32768, 60999]
13-Nov-2023 15:34:40.765 listening on IPv4 interface lo, 127.0.0.1#53
13-Nov-2023 15:34:40.765 listening on IPv4 interface eth0, 172.28.5.1#53
13-Nov-2023 15:34:40.765 generating session key for dynamic DNS
13-Nov-2023 15:34:40.765 sizing zone task pool based on 6 zones
13-Nov-2023 15:34:40.765 none:99: 'max-cache-size 90%' - setting to 14187MB (out of 15763MB)
13-Nov-2023 15:34:40.765 using built-in root key for view _default
13-Nov-2023 15:34:40.765 set up managed keys zone for view _default, file 'managed-keys.bind'
13-Nov-2023 15:34:40.765 configuring command channel from '/etc/bind/rndc.key'
13-Nov-2023 15:34:40.765 command channel listening on 127.0.0.1#953
13-Nov-2023 15:34:40.765 configuring command channel from '/etc/bind/rndc.key'
13-Nov-2023 15:34:40.765 command channel listening on ::1#953
13-Nov-2023 15:34:40.765 not using config file logging statement for logging due to -g option
13-Nov-2023 15:34:40.765 managed-keys-zone: loaded serial 2
13-Nov-2023 15:34:40.765 zone 127.in-addr.arpa/IN: loading from master file /etc/bind/db.127 failed: file not found
13-Nov-2023 15:34:40.765 zone 127.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:34:40.765 zone 0.in-addr.arpa/IN: loading from master file /etc/bind/db.0 failed: file not found
13-Nov-2023 15:34:40.765 zone 0.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:34:40.765 zone 255.in-addr.arpa/IN: loading from master file /etc/bind/db.255 failed: file not found
13-Nov-2023 15:34:40.765 zone localhost/IN: loading from master file /etc/bind/db.local failed: file not found
13-Nov-2023 15:34:40.765 zone localhost/IN: not loaded due to errors.
13-Nov-2023 15:34:40.765 zone 255.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:34:40.765 /var/lib/bind/db.tiendadeelectronica.int:13: file does not end with newline
13-Nov-2023 15:34:40.765 zone tiendadeelectronica.int/IN: NS 'db.tiendadeelectronica.int' has no address records (A or AAAA)
13-Nov-2023 15:34:40.765 zone tiendadeelectronica.int/IN: not loaded due to errors.
13-Nov-2023 15:34:40.765 all zones loaded
13-Nov-2023 15:34:40.765 running
13-Nov-2023 15:34:40.773 managed-keys-zone: No DNSKEY RRSIGs found for '.': success
13-Nov-2023 15:36:23.950 no longer listening on 127.0.0.1#53
13-Nov-2023 15:36:23.950 no longer listening on 172.28.5.1#53
13-Nov-2023 15:36:23.950 shutting down
13-Nov-2023 15:36:23.950 stopping command channel on 127.0.0.1#953
13-Nov-2023 15:36:23.950 stopping command channel on ::1#953
13-Nov-2023 15:36:23.954 automatic interface scanning terminated: end of file
13-Nov-2023 15:36:23.958 exiting
Starting named...
exec /usr/sbin/named -u "bind" "-g" ""
13-Nov-2023 15:36:24.874 starting BIND 9.18.18-0ubuntu0.23.04.1-Ubuntu (Extended Support Version) <id:>
13-Nov-2023 15:36:24.874 running on Linux x86_64 6.1.0-13-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.55-1 (2023-09-29)
13-Nov-2023 15:36:24.874 built with  '--build=x86_64-linux-gnu' '--prefix=/usr' '--includedir=${prefix}/include' '--mandir=${prefix}/share/man' '--infodir=${prefix}/share/info' '--sysconfdir=/etc' '--localstatedir=/var' '--disable-option-checking' '--disable-silent-rules' '--libdir=${prefix}/lib/x86_64-linux-gnu' '--runstatedir=/run' '--disable-maintainer-mode' '--disable-dependency-tracking' '--libdir=/usr/lib/x86_64-linux-gnu' '--sysconfdir=/etc/bind' '--with-python=python3' '--localstatedir=/' '--enable-threads' '--enable-largefile' '--with-libtool' '--enable-shared' '--disable-static' '--with-gost=no' '--with-openssl=/usr' '--with-gssapi=yes' '--with-libidn2' '--with-json-c' '--with-lmdb=/usr' '--with-gnu-ld' '--with-maxminddb' '--with-atf=no' '--enable-ipv6' '--enable-rrl' '--enable-filter-aaaa' '--disable-native-pkcs11' 'build_alias=x86_64-linux-gnu' 'CFLAGS=-g -O2 -ffile-prefix-map=/build/bind9-VMBSpu/bind9-9.18.18=. -flto=auto -ffat-lto-objects -fstack-protector-strong -Wformat -Werror=format-security -fdebug-prefix-map=/build/bind9-VMBSpu/bind9-9.18.18=/usr/src/bind9-1:9.18.18-0ubuntu0.23.04.1 -fno-strict-aliasing -fno-delete-null-pointer-checks -DNO_VERSION_DATE -DDIG_SIGCHASE' 'LDFLAGS=-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now' 'CPPFLAGS=-Wdate-time -D_FORTIFY_SOURCE=2'
13-Nov-2023 15:36:24.874 running as: named -u bind -g
13-Nov-2023 15:36:24.874 compiled by GCC 12.3.0
13-Nov-2023 15:36:24.874 compiled with OpenSSL version: OpenSSL 3.0.8 7 Feb 2023
13-Nov-2023 15:36:24.874 linked to OpenSSL version: OpenSSL 3.0.8 7 Feb 2023
13-Nov-2023 15:36:24.874 compiled with libuv version: 1.44.2
13-Nov-2023 15:36:24.874 linked to libuv version: 1.44.2
13-Nov-2023 15:36:24.874 compiled with libxml2 version: 2.9.14
13-Nov-2023 15:36:24.874 linked to libxml2 version: 20914
13-Nov-2023 15:36:24.874 compiled with json-c version: 0.16
13-Nov-2023 15:36:24.874 linked to json-c version: 0.16
13-Nov-2023 15:36:24.874 compiled with zlib version: 1.2.13
13-Nov-2023 15:36:24.874 linked to zlib version: 1.2.13
13-Nov-2023 15:36:24.874 ----------------------------------------------------
13-Nov-2023 15:36:24.874 BIND 9 is maintained by Internet Systems Consortium,
13-Nov-2023 15:36:24.874 Inc. (ISC), a non-profit 501(c)(3) public-benefit 
13-Nov-2023 15:36:24.874 corporation.  Support and training for BIND 9 are 
13-Nov-2023 15:36:24.874 available at https://www.isc.org/support
13-Nov-2023 15:36:24.874 ----------------------------------------------------
13-Nov-2023 15:36:24.874 found 12 CPUs, using 12 worker threads
13-Nov-2023 15:36:24.874 using 12 UDP listeners per interface
13-Nov-2023 15:36:24.878 DNSSEC algorithms: RSASHA1 NSEC3RSASHA1 RSASHA256 RSASHA512 ECDSAP256SHA256 ECDSAP384SHA384 ED25519 ED448
13-Nov-2023 15:36:24.878 DS algorithms: SHA-1 SHA-256 SHA-384
13-Nov-2023 15:36:24.878 HMAC algorithms: HMAC-MD5 HMAC-SHA1 HMAC-SHA224 HMAC-SHA256 HMAC-SHA384 HMAC-SHA512
13-Nov-2023 15:36:24.878 TKEY mode 2 support (Diffie-Hellman): yes
13-Nov-2023 15:36:24.878 TKEY mode 3 support (GSS-API): yes
13-Nov-2023 15:36:24.878 config.c: option 'trust-anchor-telemetry' is experimental and subject to change in the future
13-Nov-2023 15:36:24.878 loading configuration from '/etc/bind/named.conf'
13-Nov-2023 15:36:24.878 unable to open '/etc/bind/bind.keys'; using built-in keys instead
13-Nov-2023 15:36:24.878 looking for GeoIP2 databases in '/usr/share/GeoIP'
13-Nov-2023 15:36:24.878 using default UDP/IPv4 port range: [32768, 60999]
13-Nov-2023 15:36:24.878 using default UDP/IPv6 port range: [32768, 60999]
13-Nov-2023 15:36:24.878 listening on IPv4 interface lo, 127.0.0.1#53
13-Nov-2023 15:36:24.878 listening on IPv4 interface eth0, 172.28.5.1#53
13-Nov-2023 15:36:24.878 generating session key for dynamic DNS
13-Nov-2023 15:36:24.878 sizing zone task pool based on 6 zones
13-Nov-2023 15:36:24.878 none:99: 'max-cache-size 90%' - setting to 14187MB (out of 15763MB)
13-Nov-2023 15:36:24.878 using built-in root key for view _default
13-Nov-2023 15:36:24.878 set up managed keys zone for view _default, file 'managed-keys.bind'
13-Nov-2023 15:36:24.882 configuring command channel from '/etc/bind/rndc.key'
13-Nov-2023 15:36:24.882 command channel listening on 127.0.0.1#953
13-Nov-2023 15:36:24.882 configuring command channel from '/etc/bind/rndc.key'
13-Nov-2023 15:36:24.882 command channel listening on ::1#953
13-Nov-2023 15:36:24.882 not using config file logging statement for logging due to -g option
13-Nov-2023 15:36:24.882 managed-keys-zone: loaded serial 3
13-Nov-2023 15:36:24.882 zone 127.in-addr.arpa/IN: loading from master file /etc/bind/db.127 failed: file not found
13-Nov-2023 15:36:24.882 zone 127.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:36:24.882 zone 0.in-addr.arpa/IN: loading from master file /etc/bind/db.0 failed: file not found
13-Nov-2023 15:36:24.882 zone 0.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:36:24.882 zone 255.in-addr.arpa/IN: loading from master file /etc/bind/db.255 failed: file not found
13-Nov-2023 15:36:24.882 zone 255.in-addr.arpa/IN: not loaded due to errors.
13-Nov-2023 15:36:24.882 zone localhost/IN: loading from master file /etc/bind/db.local failed: file not found
13-Nov-2023 15:36:24.882 zone localhost/IN: not loaded due to errors.
13-Nov-2023 15:36:24.882 /var/lib/bind/db.tiendadeelectronica.int:13: file does not end with newline
13-Nov-2023 15:36:24.882 zone tiendadeelectronica.int/IN: loaded serial 10000002
13-Nov-2023 15:36:24.882 all zones loaded
13-Nov-2023 15:36:24.882 running
13-Nov-2023 15:36:24.886 managed-keys-zone: No DNSKEY RRSIGs found for '.': success
13-Nov-2023 15:37:27.281 no valid RRSIG resolving 'com/DS/IN': 8.8.8.8#53
13-Nov-2023 15:37:27.289 no valid RRSIG resolving 'com/DS/IN': 1.1.1.1#53
13-Nov-2023 15:37:27.289 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:37:27.289 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:37:28.309 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:28.309 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:37:28.313 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:28.313 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:37:30.321 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:30.321 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:30.325 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:30.325 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:34.329 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:34.329 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:34.333 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:34.333 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:48.298 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:48.298 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:48.302 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:48.302 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:49.310 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:49.310 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:49.314 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:49.314 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:51.346 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:51.346 broken trust chain resolving 'security.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:37:51.350 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:37:51.350 broken trust chain resolving 'archive.ubuntu.com/A/IN': 8.8.8.8#53
13-Nov-2023 15:39:49.952 no valid RRSIG resolving 'com/DS/IN': 1.1.1.1#53
13-Nov-2023 15:39:49.960 no valid RRSIG resolving 'com/DS/IN': 8.8.8.8#53
13-Nov-2023 15:39:49.960 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:39:49.960 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:39:50.972 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:39:50.972 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:39:50.976 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:39:50.976 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:15.445 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:40:15.445 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:15.449 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:40:15.449 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:16.453 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:40:16.453 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:16.457 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:40:16.457 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:18.465 validating security.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:40:18.465 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:18.469 validating archive.ubuntu.com/A: bad cache hit (com/DS)
13-Nov-2023 15:40:18.469 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:22.493 no valid RRSIG resolving 'com/DS/IN': 1.1.1.1#53
13-Nov-2023 15:40:22.497 no valid RRSIG resolving 'com/DS/IN': 8.8.8.8#53
13-Nov-2023 15:40:22.497 broken trust chain resolving 'archive.ubuntu.com/A/IN': 1.1.1.1#53
13-Nov-2023 15:40:22.497 broken trust chain resolving 'security.ubuntu.com/A/IN': 1.1.1.1#53
```

## 10.Realiza el apartado 9 en la máquina virtual con DNS
