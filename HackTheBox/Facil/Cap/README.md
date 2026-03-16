<center><h1>Cap</h1></center>  
<p align="center">

<img src="img/banner.png" width="400">


## ❓ ¿Qué es Cap?

Cap es una máquina Linux enfocada en enumeración de servicios, explotación de un fallo IDOR y análisis de tráfico de red para obtener credenciales. Permite practicar acceso inicial mediante servicios remotos y escalada de privilegios abusando de capacidades especiales en binarios del sistema.



## 🔝 Despliegue Cap

Tras descargar el archivo ovpn que ofrece la plataforma, es necesario en terminal ejecutar el comando **sudo openvpn archivo vpn**

![Conexión VPN](img/conexion_vpn.png)

Además, es necesario comprobar conectividad entre nuestra máquina kali y el objetivo

![Conectividad](img/conectividad.png)



## 🔎 Fase de Descubrimiento 
Ahora, se abrirá una nueva terminal para empezar a realizar el descubrimiento del sistema. Cómo sabemos la dirección IP de la máquina vulnerable **(10.129.9.139)**, comenzaremos realizando un escaneo de red nmap. 
En esta ocación, se usará el comando **nmap -sC -sV --min-rate 5000 10.129.9.139 -oN escaneo.txt**


| Argumento | Significado |
|---|---|
| -sC | Ejecuta los scripts para comprobaciones comunes |
| -sV | Detección de versiones de servicios |
| --min-rate 5000 | Envía al  5000 paquetes por segundo (aumenta velocidad; puede causar pérdida o detección) |
| 10.129.9.139| Dirección IP del objetivo a escanear |
| -oN escaneo.txt | Guarda la salida en el fichero escaneo.txt |

![Escaneo](img/nmap.png)

> [!NOTE]
>
>Se ha realizado un escaneo agresivo debido a que se está realizando en un entorno controlado y no es importante el ser detectado. Si se busca hacer el mínimo ruido posible será necesario utilizar el argumento **-sS** se usa para no ser detectado fácilmente, porque no completa la conexión TCP. Además, **no se usará --min-rate.** 


En este caso, se ha encontrado los siguientes servicios TCP:
- **FTP (Puerto:21):** Intercambio de ficheros
- **SSH (Puerto: 22):** Conexión remota.
- **HTTP (Puerto 80):** Servidor web.

Con esta información permite responder a la pregunta **¿Cúantos puertos TCP hay abiertos?**
![Primera respuesta](img/primera.png)


A continuación, se dispone a visitar la página web, se encuentra lo siguiente:
![Página web](img/web.png)

Vemos que estamos logueados con la cuenta de nathan.

A continuación, al entrrara a Security Snapshot, dirige a la carpeta `/data/id` dónde el id incrementa cada veaz que refresque la página.

![Security SnapShot](img/security.png)

Con esta información se puede responder a la segunda pregunta: **Después de ejecutar un "Security Snapshot", el navegador se redirige a una ruta del formato /[algo]/[id], donde [id] representa el número de identificación del escaneo. ¿Cuál es el [algo]?**

![Segunda](img/segunda.png)

Además, podemos responder la tercera pregunta: **¿Puedes acceder a los escaneos de otros usuarios?**

![Tercera](img/tercera.png)

Se ha identificado una vulnerabilidad de tipo **IDOR (Insecure Direct Object Reference)** en la ruta **data/[id]**, ya que modificando manualmente el identificador es posible acceder a escaneos almacenados sin autorización.  

Los snapshots con ID **3** y **1** corresponden a datos creados por mi usuario, mientras que el ID **0** pertenece a otro usuario, lo que demuestra que **sí es posible acceder a los escaneos de otros usuarios** manipulando el ID.  

A continuación, se muestran los resultados:

Snapshots creados por mi usuario:  

![Acceso datos 3](img/datos_3.png)  
![Acceso datos 1](img/datos_1.png)  

Datos de otro usuario:  

![Acceso datos 0](img/datos_0.png)

Con esta información se puede responder a la cuarta pregunta:**¿Cuál es el ID del archivo PCAP que contiene datos sensibles?**

![Cuarta](img/cuarta.png)


Tras obtener el ID 0 (del usuario desconocido), se procede a descargar el fichero .pcap, dónde se abrirá con wireshark para analizar dichos paquetes.

En él se encuentra peticiones FTP con las siguientes credenciales:

- Usuario: nathan
- Contraseña: Buck3tH4TF0RM3!

![credenciales FTP](img/ftp.png)

Gracias a esta información, se puede responder a la quinta:

**¿En qué protocolo de la capa de aplicación del archivo PCAP se pueden encontrar los datos sensibles?**

![Quinta](img/quinta.png)


## 🖥️ Acceso al servidor
Cómo se tiene las credenciales del usuario nathan, se puede acceder al sistema mediante FTP. Dónde encontramos un archivo (**dir**) llamado **user.txt**, que será la flag.

![Acceso FTP](img/acceso_ftp.png)

También se puede acceder al servicio SSH. Es recomentable intentar acceder a todos los servicios con las credenciales obtenidas preivamente.

![Acceso SSH](img/acceso_ssh.png)

Con esta información se puede responder a la pregunta **Hemos conseguido averiguar la contraseña FTP de Nathan. ¿En qué otros servicios funciona esta contraseña?**.

![Sexta pregunta](img/sexta.png)

## 🔓 Escalada de privilegios

Como no tengo sudo (**sudo -l** no muestra nada), utilizo **getcap -r /**:

![Capability](img/getcap.png)

En la imagen anterior se muestra que **Python (/usr/bin/python3.8) tiene capacidades especiales** (**cap_setuid, cap_net_bind_service+eip**).  
Esto significa que podemos ejecutar Python con **permisos ampliados**, lo que podría permitirnos **escalar privilegios** incluso sin sudo.


Con esta información se puede responder la octava pregunta: **¿Cuál es la ruta completa al archivo binario de este equipo que tiene capacidades especiales que pueden ser objeto de abuso para obtener privilegios de root?**

![Octava](img/octava.png)

Para ganar acceso root, se ejecuta el binario python3.8 para abrir una terminal de Python, desde donde se importa el módulo os, se establece el identificador de usuario a 0 (root) mediante os.setuid(0) y posteriormente se abre una shell bash con privilegios elevados.

![Acceso root](img/acceso_root.png)

La flag está localizada en **/root/root.txt**

![Flag Root](img/flag_root.png)


## <img src="./img/logoactual.jpg" width=25 style="; border-radius:50%;">  ¡Hola! Me llamo Saúl Ruiz 
### Estudiante en Ciberseguridad


![YouTube](https://img.shields.io/youtube/channel/subscribers/UCcOkvgreZrXauRHyXlii0JA)
![Seguidores](https://img.shields.io/github/followers/saulruizplaza)
[![Twitter Follow](https://img.shields.io/twitter/follow/plasysx?style=social)](https://twitter.com/plasysx)

Soy estudiante de Administración de Sistemas Informáticos en Red con pasión por la ciberseguridad y el mundo de la informática. Desde pequeño disfruto explorando tecnología y aprendiendo de manera autónoma. Además, combino mis estudios con la creación de contenido y recursos educativos sobre informática a través de mi proyecto personal <b>[@PlaSysX](https://linktr.ee/PlaSysx)</b>

Si quieres aprender informática, mejorar tus habilidades, descubrir trucos y soluciones prácticas, y formar parte de nuestra comunidad, puedes seguirnos en PlaSysX.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Saúl_Ruiz_Plaza-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=101010)](https://www.linkedin.com/in/saulruizplaza) [![Instagram](https://img.shields.io/badge/Instagram-@PlaSysX-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://instagram.com/plasysx)
[![TikTok](https://img.shields.io/badge/TikTok-@plasysx_es-69C9D0?style=for-the-badge&logo=tiktok&logoColor=white&labelColor=101010)](https://tiktok.com/@plasysx_es)
[![YouTube](https://img.shields.io/badge/YouTube-Plasysx-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://youtube.com/@Plasysx)
[![Twitter](https://img.shields.io/badge/Twitter-@plasysx-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=101010)](https://twitter.com/plasysx)
