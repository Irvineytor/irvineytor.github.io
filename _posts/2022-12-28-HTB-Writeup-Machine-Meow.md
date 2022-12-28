---
title: HTB - Writeup - Meow  
published: true
---

![](/assets/HTB-Machine-Meow-1.png)

A continuacion estaremos siguiendo el paso a paso (writeup) para resolver una maquina "Very Easy" que forma parte del "Starting Point - Tier 0" de HTB.

En esta maquina iniciaremos la exploracion de herramientas como:
* `ping` - Es una utilidad de comando sencilla que nos ayuda a verificar si se tiene comunicacion con alguna IP proporcionada por medio de envio de paquetes ICMP.
* `nmap` - Con los diferentes parametros que le podemos asignar para un mejor despliegue de informacion.
* `Telnet` - (TErminaL NETwork) Es un tipo de protocolo que permite que una computadora se conecte a una computadora local. Usa el protocolo de red TCP/IP para crear sesiones remotas de manera muy facil. Telnet no es un protocolo seguro de comunicacion y no esta cifrado.


## [](#header-2) Resolucion de la maquina "Meow"
* * *
### [](#header-3) Primeros pasos:


Lo primero que hay que hacer es asegurarnos de que tenemos descargada nuestra VPN de HTB, si no sabes como hacerlo, mira el anterior post en donde explico que es HTB y como montar tu VPN [(Miralo aqui)](/What-is-HTB).


Una vez hecho el comando:
```bash
$ sudo openvpn filename.ovpn
```

Volveremos al ["Starting Point"](https://app.hackthebox.com/starting-point) de HTB en donde nos aparecera que la conexion al Starting Point esta hecha y un boton de ***SPAWN MACHINE*** para encender la maquina en cuestion.


![](/assets/HTB-Writeup-Machine-Meow/openvpn-succefully.png)


Sera cuestion de esperar un par de minutos a que la maquina encienda y podamos visualizar una direccion IP similar a la que muestra en la siguiente imagen:

![](/assets/HTB-Writeup-Machine-Meow/machine-on-ip.png)

Una vez visualizada la direccion IP, podemos comenzar a jugar con la maquina.

* * *
### [](#header-3) Fase de reconocimiento:

Comenzaremos empleando una herramienta basica para confirmar que tengamos conexion a la maquina. Esto lo podemos hacer muy facil con el uso de ```ping``` usandolo de la siguiente manera.
```bash
$ ping -c 3 10.129.112.207
```
> El uso de la marca ```-c 3```, indica que solo se desea hacer el envio de 3 conjuntos de paquetes a la direccion espesificada. Si no se usa esta flag, la herramienta enviara constantemente paquetes a la direccion hasta que hagamos una interrupcion por teclando haciendo uso de ```CTRL + C```

Una vez terminada la ejecucion del comando anterior, la consola nos va a imprimir algo como esto:
```bash
$ ping -c 3 10.129.112.207

PING 10.129.112.207 (10.129.112.207) 56(84) bytes of data.
64 bytes from 10.129.112.207: icmp_seq=1 ttl=63 time=60.9 ms
64 bytes from 10.129.112.207: icmp_seq=2 ttl=63 time=60.9 ms
64 bytes from 10.129.112.207: icmp_seq=3 ttl=63 time=61.0 ms

--- 10.129.112.207 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 60.919/60.959/61.016/0.041 ms

$ 

```

>Lo que nos estara dando informacion relevante como el ttl, la estabilidad de la comunicacion, etc.
>El ttl nos indica a que tipo de kernel nos estamos enfrentando, normalmente si el ttl esta al rededor de los 60-70 = ttl, es un indicador de que nos estamos enfrentando a un sistema Linux, por otra parte cuando el ttl ronda los 110-125 = ttl, podemos asumir que nos estamos enfrentando a una maquina Windows.

* * *
### [](#header-3) Escaneo de puertos:

Para este paso hay una herramienta de gran utilidad, estamos hablando de ```nmap```. Llamamos a la herramienta y aputamos a la direccion a la que queremos dirigir el escaneo.

```bash
$ sudo nmap -v 10.129.112.207
```
Una ves que haya terminado la herramienta de ejecutar el escaneo, nos mostrara en consola un resumen de informacion sobre la direccion a la que hemos apuntado y nos enlistara los puertos que haya encontrado abiertos junto a su servicio:

![](/assets/HTB-Writeup-Machine-Meow/nmap-result.png)

Esto nos dice que tiene abierto el puerto 23/tcp y que el servicio que esta corriendo por ese puerto es el telnet, como habiamos comentado antes, Telnet es una buena opcion de comunicacion entre maquinas locales, la desventaja es que este protocolo de comunicacion no esta cifrado y podemos abusar de eso como veremos a continuacion.


* * *
### [](#header-3) Conectando a Telnet:

Para conectarnos por medio del servicio de Telnet, lo haremos de la siguiente manera:
```bash
$ telnet 10.129.112.127
```
Nos mostrara la pantalla de login de telnet en donde nos estara pidiendo un usuario:

![](/assets/HTB-Writeup-Machine-Meow/telnet-conection.png)


Como primer intento, ingresaremos las credenciales que vienen por defecto en el servicio telnet
* ```Usuario``` - root
* ```Key``` - Change_me123


Observamos que las credenciales no fueron modificadas por parte del admin TI y hemos logrado el acceso al servicio.
> En el caso donde el usuario estuviera equivocado, podriamos intentar con __admin__ o __administrator__.

* * *
### [](#header-3) Escalada de privilegios y Flag:

Al acceder por el servicio Telnet con las credenciales anteriores, nos mostrara por consola que nos estamos comunicando con una maquina Ubuntu 20.04.2 LTS y que estamos logeados como usuario root dentro de la maquina Meow
```bash
$ root@Meow:~# 
```

Por lo que simplemente podemos listar los archivos del directorio acual con:
```bash
root@Meow:~# ls
```
Hecho esto observamos un archivo tipico en los CTF (Capture The Flag):
```bash
root@Meow:~# ls
flag.txt snap
```
Con lo cual para observar la Flag para ingresarla en HTB y comprobar que hemos podido pwnear el sistema, basta con hacer un cat al archivo de la siguiente manera:
```bash
root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
```
Ahora solo falta ingresar la Flag que copiamos en el campo de HTB y habremos terminado nuestra primera maquina en HTB.
Como complemento HTB ofrece una serie de preguntas que te iran dando algunas pistas en este Starting Point, algunas ya las hemos contestado en el transcurso de este blog, aun que de igual forma, haremos una seccion al final de este para recopilar todas las preguntas con su respectiva respuesta. 

Espero que esta guia te haya ayudado y hayas aprendido algo nuevo, gracias por llegar hasta aqui!

## [](#header-2) Resolucion de preguntas
* * *
### [](#header-3) Pregunta 1: Que significa el acronimo VM?

* Virtual Machine
> Virtual Machine, o maquina virtual, es un entorno virtual que funciona como un sistema informatico virtual con su propia CPU, memoria, interfaz de red y almacenamiento, creado en un sistema de hardware fisico.


### [](#header-3) Pregunta 2: ¿Qué herramienta utilizamos para interactuar con el sistema operativo con el fin de emitir comandos a través de la línea de comandos, como el de iniciar nuestra conexión VPN? También se conoce como consola o caparazón.

* Terminal
> La terminal es una interfaz basada en texto que se utiliza para controlar una computadora, mediante la ejecucion de un comando con la entrada adecuada.


### [](#header-3) Pregunta 3: ¿Qué servicio usamos para formar nuestra conexión VPN en los laboratorios HTB?

* OpenVPN
> OpenVPN, es un VPN de codigo abierto que utiliza tecnicas de red privada virtual (VPN), para establecer conexiones seguras de punto a punto.



### [](#header-3) Pregunta 4: ¿Cuál es el nombre abreviado de una 'interfaz de túnel' en el resultado de la secuencia de arranque de su VPN?

*TUN
>Los dispositivos TUN son interfaces virtuales que utilizan los clientes VPN para establecer instancias virtuales de conexiones de redes fisicas. TUN (TUNel de red) simula un dispositivo de capa de red y opera en la capa 3 transportando paquetes IP.

### [](#header-3) Pregunta 5: ¿Qué herramienta usamos para probar su conexión con el objetivo con una solicitud de eco ICMP?

*PING
>Ping (Packet Internet Groper), usa ICMP (Protocolo de Mensajes de Control de Internet) proporciona informacion relacionada con el procesamiento de paquetes IP. Ping funciona enviando un mensaje de solicitud de eco ICMP a la direccion IP proporcionada. Si se puede acceder a la computadora con la direccion IP de destino, responde con un mensaje de respuesta de eco ICMP.

### [](#header-3) Pregunta 6: ¿Cuál es el nombre de la herramienta más común para encontrar puertos abiertos en un objetivo?

*NMAP
>Nmap (Network Mapper), es una herramienta gratuita y de codigo abierto para el descubrimiento de redes y la auditoria de seguridad. 
>Se utiliza para descubrir hists y servicios en una red informatica mediante el envio de paquetes y el analisis de las respuestas.


### [](#header-3) Pregunta 7: ¿Qué servicio identificamos en el puerto 23/tcp durante nuestros escaneos?

*Telnet
>Telnet (TErminal NETwork) es un tipo de protocolo que permite que una computadora se conecte a una computadora local. Sigue el protocolo de red TCP/IP para crear sesiones remotas. Telnet no es un protocolo seguro y no esta cifrado.


### [](#header-3) Pregunta 8: ¿Qué nombre de usuario puede iniciar sesión en el objetivo a través de telnet con una contraseña en blanco?

*root
>Root es el usuario mas comun dentro de las credenciales por defecto. En caso de que este pueda no funcionar, tambien se puede probar con usuarios como admin, administrador, etc.


