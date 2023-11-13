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
   

## 10.Realiza el apartado 9 en la máquina virtual con DNS
