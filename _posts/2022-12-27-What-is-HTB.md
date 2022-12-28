---
title: Que es Hack the Box? (HTB) 
published: true
---

![](assets/What-is-HTB/HTB-top-banner.png)


Bienvenido a mi primer post!
Ahora estaremos comentando un poco acerca de la plataforma Hack the Box.

Imagina estar conectado a una red que promueva el hackeo etico y te ponga en un ranking con otros cientos de miles de hackers, suena cuanto menos interesante, no?

"Hack the Box" por sus siglas (HTB), es una plataforma online que brinda esta oportunidad de manera completamente legal y en una parte gratis por medio de su VPN totalmente segura.
Ofrece una red de maquinas diseñadas para ser vulneradas y te permite practicar tus habilidades de ciberseguridad para demotrar tus habilidades como "PEN Tester", comprobando que hayas vulnerando la maquina por medio de "Flags" que se encuentran escondidas en cada maquina, teniendo que llegar a obtener privilegios de usuario "root" para acceder a ellas. Las maquinas intentan simular servidores reales, con paginas web, servicios de administracion y servidores de dominio, entre otras caracteristicas. Para acceder como "root", tendras que utilizar tecnicas de reconocimiento de puertos, simulacion de ataques reales, escalacion de privilegios, esteganografia, fuerza bruta y analisis de codigo. Si necesitas ayuda para comenzar, te recomiendo que te quedes por mi pagina y revises las publicaciones diarias, tratare de ir subiendo soluciones a maquinas, paso a paso, lo mejor explicadas de mi parte posible para que puedas comprender las vulnerabilidades que se trabajaran y como se logra acceder a maquinas atraves de sus servicios y puertos expuestos.1



## [](#header-2) Comienzo en HTB
* * *
### [](#header-3) Entorno de trabajo


Lo primero que recomiendo es montar un laboratorio con un entorno o sistema operativo basado en Linux, para esto hay varias distribuciones de Linux especializadas en la ciberseguridad y las pruebas de penetracion, talex como:


* [KaliOS](https://www.kali.org/)


* [ParrotOS](https://www.parrotsec.org/)


* [BlackArch](https://blackarch.org/)

...entre otros, en cualquier caso, puedes elegir la distribucion de linux que mas te agrade y te sea mas facil de manejar, en mi experiencia personal te puedo recomendar [KaliOS](https://www.kali.org/), puedes montar el sistema operativo ya sea nativo en tu PC, o bien puedes contar con alternativas como correrlo desde un LiveUSB o una maquina virtual, cada una de estas opciones cuenta con sus ventajas y desventajas como te podrias imaginar, asi que es cuestion de decidirse por alguna de ellas y comenzar manos a la obra!



* * *
### [](#header-3) Creando una cuenta en HTB


Por fortuna, en esta plataforma hay un numero de maquinas que podemos acceder de manera gratuita, ya que existe una version de pago mensual en la que podemos acceder a un conjunto mas de maquinas retiradas.

Dejemos los lios, el primer paso es:
* Crear una cuenta en [Hack the Box](https://app.hackthebox.com/login)
> Una vez tengamos nuestra cuenta creada y confirmada accederemos a la pagina de inicio. Podremos observar en la pagina superior derecha que hay un boton llamado "Connect to HTB", se nos despliega un menu y podremos descargar nuestra VPN personalizada.

* Instalar la herramienta OPENVPN:
> Para hacer uso de OPENVPN es necesario tener la herramienta descargada, en linux es tan facil como hacer un:


```bash
$ sudo apt-get install -y openvpn
```
* Montar nuestra VPN de HTB:
> Luego de haber instalado la herramienta OPENVPN, podemos montar nuestra VPN de HTB con el siguiente comando:


```bash
$ sudo openvpn name_of_your_vpn.ovpn
```


* Acceder a una maquina:
> Es hora de seleccionar una maquina, para comenzar iniciaremos por el ["Starting Point"](https://app.hackthebox.com/starting-point) de HTB que se encuentra en la pestaña "Labs" que se encuentra a la izquierda de la pagina principal.

* Encender la maquina:
> Ahora es cuestion de buscar la opcion de "Click to Spawn the machine". Esto nos generara una IP (129.xx.xxx.xx) con la que podremos acceder a la maquina en cuestion, ahora solo queda comenzar a jugar, contestar las preguntas que nos iran guiando a la resolucion de la maquina e ingresar la "flag" para comprobar que hemos pwneado la maquina.
