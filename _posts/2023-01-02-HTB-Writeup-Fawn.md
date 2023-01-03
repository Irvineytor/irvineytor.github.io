---
title: HTB - Writeup - Fawn 
published: true
---

![](/assets/HTB-Writeup-Machine-Fawn/machine-fawn.png)


Despues de esta temporada de fiestas familiares y mucha comida, toca ponerse manos a la obra y continuar aprendiendo. Para la maquina de hoy, tomaremos la maquina numero dos del ["Starting Point - Tier 0"](https://app.hackthebox.com/starting-point) de HTB, formando parte del conjunto de maquinas "Very Easy" para la familiarizacion con la plataforma y conceptos basicos sobre la ciberseguridad, estamos hablando de la maquina "Fawn".


En esta maquina haremos uso de herramientas ya vistas y algun protocolo nuevo:
* `ping` - Es una utilidad de comando sencilla que nos ayuda a verificar si se tiene comunicacion con alguna IP proporcionada por medio de envio de paquetes ICMP.
* `nmap` - Con los diferentes parametros que le podemos asignar para un mejor despliegue de informacion.
* `FTP` - FTP (File Transfer Protocol) es un protocolo de red que se utiliza para intercambiar archivos a través de una red de ordenadores. Se puede usar para subir o descargar archivos a un servidor de internet o para compartir archivos entre dos ordenadores de forma local. FTP es una forma muy común de intercambiar archivos y es compatible con la mayoría de los sistemas operativos.

### [](#header-3) Un poco sobre FTP:


FTP no es un protocolo seguro por sí mismo. La información que se envía a través de FTP, incluyendo nombres de usuario y contraseñas, se envía sin cifrar a través de la red. Esto significa que es posible que otras personas puedan interceptar la información durante la transmisión y utilizarla de forma indebida.

Para mejorar la seguridad del uso de FTP, hay varias opciones que puedes considerar:

1. Utilizar SFTP (SSH File Transfer Protocol) en lugar de FTP. SFTP utiliza cifrado para proteger la información que se envía a través de la red.

1. Utilizar una conexión segura (SSL) para encriptar la información que se envía a través de FTP. Esto se conoce como FTPS (FTP seguro).

1. Utilizar una red privada virtual (VPN) para proteger tu conexión a Internet y mejorar la seguridad de la transmisión de datos.

1. Es importante tener en cuenta que, aunque estas opciones pueden mejorar la seguridad del uso de FTP, no son infalibles. Es importante tomar medidas adicionales para proteger tu información y garantizar la seguridad de tus archivos.


## [](#header-2) Resolucion de la maquina "Fawn"
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

![](/assets/HTB-Writeup-Machine-Fawn/fawn-ip.png)

Una vez visualizada la direccion IP, podemos comenzar a jugar con la maquina.

* * *
### [](#header-3) Fase de reconocimiento:

Comenzaremos empleando una herramienta basica para confirmar que tengamos conexion a la maquina. Esto lo podemos hacer muy facil con el uso de ```ping``` usandolo de la siguiente manera.
```bash
$ ping -c 3 10.129.54.239
```
> El uso de la marca ```-c 3```, indica que solo se desea hacer el envio de 3 conjuntos de paquetes a la direccion espesificada. Si no se usa esta flag, la herramienta enviara constantemente paquetes a la direccion hasta que hagamos una interrupcion por teclando haciendo uso de ```CTRL + C```

Una vez terminada la ejecucion del comando anterior, la consola nos va a imprimir algo como esto:
```bash
$ ping -c 3 10.129.54.239

PING 10.129.112.207 (10.129.112.207) 56(84) bytes of data.
64 bytes from 10.129.54.239: icmp_seq=1 ttl=63 time=60.9 ms
64 bytes from 10.129.54.239: icmp_seq=2 ttl=63 time=60.9 ms
64 bytes from 10.129.54.239: icmp_seq=3 ttl=63 time=61.0 ms

--- 10.129.54.239 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 60.919/60.959/61.016/0.041 ms

$ 

```

>Lo que nos estara dando informacion relevante como el ttl, la estabilidad de la comunicacion, etc.
>El ttl nos indica a que tipo de kernel nos estamos enfrentando, normalmente si el ttl esta al rededor de los 60-70 = ttl, es un indicador de que nos estamos enfrentando a un sistema Linux, por otra parte cuando el ttl ronda los 110-125 = ttl, podemos asumir que nos estamos enfrentando a una maquina Windows.

* * *
### [](#header-3) Escaneo de puertos:

Para este paso hay una herramienta de gran utilidad que ya hemos usado, estamos hablando de ```nmap```. Llamamos a la herramienta y aputamos a la direccion a la que queremos dirigir el escaneo.

```bash
$ sudo nmap -v 10.129.54.239
```
Una ves que haya terminado la herramienta de ejecutar el escaneo, nos mostrara en consola un resumen de informacion sobre la direccion a la que hemos apuntado y nos enlistara los puertos que haya encontrado abiertos junto a su servicio:

![](/assets/HTB-Writeup-Machine-Fawn/nmap-result.png)

Esto nos dice que tiene abierto el puerto 21/tcp y que el servicio que esta corriendo por ese puerto es el vsftpd 3.0.3:


* * *
### [](#header-3) Conectando a FTP:

Para conectarnos por medio del servicio de FTP, lo haremos de la siguiente manera:
```bash
$ ftp -v 10.129.54.239
```
Nos mostrara la pantalla de login de ftp en donde nos estara pidiendo un usuario y luego pedira que especifiquemos una contraseña:


Como primer intento, ingresaremos las credenciales por defecto mayormente usadas para este servicio:

* `Usuario` - anonymous
* `Key` - anonymous

![](/assets/HTB-Writeup-Machine-Fawn/ftp-conection.png)


Observamos que las credenciales fueron aceptadas y estamos dentro del servicio una vez que nos haya devuelto el codigo `230 Login successful`.


* * *
### [](#header-3) Escalada de privilegios y Flag:

Al acceder por el servicio `ftp` con las credenciales anteriores, nos mostrara por consola que nos estamos comunicando con una maquina UNIX y podemos hacer comandos `bash` dentro de la maquina Fawn
```bash
$ ftp> 
```

Por lo que simplemente podemos listar los archivos del directorio acual con:
```bash
$ ftp> ls
```
Hecho esto observamos un archivo tipico en los CTF (Capture The Flag):

![](/assets/HTB-Writeup-Machine-Fawn/ftp-ls.png)


Ahora podremos hacer un `get flag.txt` para descargar el CTF, esto descargara el archivo .txt en nuestro directorio personal y ahi podremos observar nuestra flag

```bash
$ ftp> get flag.txt
```
Una vez descagada nuestra CTF, salimos con `exit` y vamos a nuestro directorio personal (/home/user/), al hacer un `ls` podremos observar que ya contamos con nuestro .txt

![](/assets/HTB-Writeup-Machine-Fawn/ls-flag.png)

Para listar el contenido de la flag.txt hacemos un cat al archivo para imprimir en consola el contenido de la flag:

```bash
$ cat flag.txt
035db21c881520061c53e0536e44f815  
```

Y estariamos visualizando finalmente la flag, ahora solo es cuestion de copiarla y pegarla en nuestra maquina de HTB para comprobar que hemos logrado Pwnear la maquina.


Espero que esta guia te haya ayudado y hayas aprendido algo nuevo, gracias por llegar hasta aqui!
Proximamente estare adjuntando las respuestas y explicaciones a las preguntas de HTB para esta maquina, un saludo y nos estamos leyendo pronto!
