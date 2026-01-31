<center><h1>Tproot</h1></center>  
<p align="center">

![Banner](img/banner.png)


## ❓ ¿Qué es tproot?

tproot es una máquina vulnerable orrada en la enumeración de servicios y la explotación de vulnerabilidades conocidas en sistemas Linux. Mediante el análisis de servicios expuestos como FTP y HTTP, el atacante podrá identificar software vulnerable y obtener acceso remoto al sistema.

La máquina destaca por el uso de VSFTPD 2.3.4, que contiene una puerta trasera explotable, permitiendo conseguir una shell con privilegios root de forma directa. Está pensada para quienes se inician en el pentesting y desean comprender la importancia de la enumeración y el análisis de versiones de servicios.

> [!NOTE]
>
>Puede descargar la máquina a través del **[enlace mega](https://mega.nz/file/ORUEzLia#WQgvveTv3kAnXBs6UyRShd1JomGNg6Sk7DSa_fJwD7k)**


## 🔝 Despliegue tproot

Al descargar la máquina, es necesario descompromirlo para poder encontrar los archivos necesarios para poder desplegarla, para ello, utilizaremos el comando.

**unzip tproot.zip.**

![unzip vacaciones.zip](img/unzip.png)

Obtendremos dos ficheros:
- **Auto_deploy.sh:** Script Bash para desplegar nuestra máquina localmente.
- **tproot.tar:** Máquina vulnerable contenizada.

Para desplegar el servicio será necesario carle permisos de ejecución a auto_deploy.sh, ya que por defecto tiene permisos 644. Para ello, usaremos el comando:

 **chmod +x auto_deploy.sh**

 Una vez ejecutado, se utilizará el comando **./auto_deploy.sh vacaciones.tar** para lanzar la máquina

![Despliegue](img/despliegue.png)


## 🔎 Fase de Descubrimiento 
Ahora, se abrirá una nueva terminal para empezar a realizar el descubrimiento del sistema. Cómo sabemos la dirección IP de la máquina vulnerable **(172.17.0.2)**, comenzaremos realizando un escaneo de red nmap. 
En esta ocación, se usará el comando **nmap -sC -sV --min-rate 5000 172.17.0.2**


| Argumento | Significado |
|---|---|
| -sC | Ejecuta los scripts para comprobaciones comunes |
| -sV | Detección de versiones de servicios |
| --min-rate 5000 | Envía al  5000 paquetes por segundo (aumenta velocidad; puede causar pérdida o detección) |
| 172.17.0.2 | Dirección IP del objetivo a escanear |


![Escaneo](img/nmap.png)

> [!NOTE]
>
>Se ha realizado un escaneo agresivo debido a que se está realizando en un entorno controlado y no es importante el ser detectado. Si se busca hacer el mínimo ruido posible será necesario utilizar el argumento **-sS** se usa para no ser detectado fácilmente, porque no completa la conexión TCP. Además, **no se usará --min-rate.**


En este caso, se ha encontrado dos servicios activos:
- **FTP (Puerto: 21):** Intercambios de ficheros. Si se accede con credenciales **anonymous** / **anonymous**, se recibe un error del servidor **500**
- **HTTP (Puerto 80):** Servidor Web.

A continuación, se procede a visitar el sitio web utilizando el protocolo http, que se encuentra la página de apache2

![Vista en la web](img/webview.png)


No se encuentra, ningún dato relevante para nuestra investigación, se procede a utilizar nmap para identificar vulnerabilidades. **nmap --min-rate 5000 --script=vuln 172.17.0.2**

| Argumento | Significado |
|---|---|
| --min-rate 5000 | Envía al menos 5000 paquetes por segundo |
| --script=vuln | Ejecuta scripts de detección de vulnerabilidades |
| 172.17.0.2 | Dirección IP del objetivo a escanear |

El resultado que se obtiene es que el servicio ftp tiene una vulnerabilidad explotable denominada backdoor, esto significa que el servicio VSFTPD 2.3.4 contiene una puerta trasera que permite ejecutar comandos remotos. Un atacante puede conectarse al servicio FTP y, aprovechando esta vulnerabilidad, obtener acceso de shell al sistema sin necesidad de credenciales válidas, lo que facilita el acceso inicial a la máquina objetivo.

![script vuln](img/vuln.png)


Para buscar algún exploit para ftp, se utilizará metaexploit ejecutando **msfconsole** en la terminal. Posteriormente, con search vsftpd 2.3.4 encontraremos el exploit que se requiere.

![Busqueda metaexploit](img/metaexploit.png)

Este trata de acceso por puerta trasera

## 🖥️ Acceso al servidor

Para usar el exploit que se ha encontrado, es necesario utilizar el comando **use 0**, ya que cuenta con el id 0.

En metaexploit, se requiere configurar algunas opciones para poder ejecutar nuestro exploit, para ello se usa el comando **show options**

| Opción | Valor | Descripción |
|---|---|---|
| RHOSTS | 172.17.0.2 | Dirección IP de la máquina víctima |
| RPORT | 21 | Puerto de la máquina víctima |
| CHOST | 10.0.2.15 | Dirección IP de nuestra máquina (atacante) |
| CPORT | 5000 | Puerto de escucha para la conexión inversa |

![Show options](img/options.png)

Se requiere introducir en RHOSTS la dirección ip de la máquina victima usando el comando **set RHOSTS 172.17.0.2**

![RHOSTS](img/rhosts.png)

Será necesario introducir la dirección de nuestra máquina utilizando el comando **set CHOST 10.0.2.15** y un puerto que se utilizará de escucha usando el comando **set CPORT 5000**.


![Show option final](img/options_final.png)

Por último, en metaexpoloit, se ejecutará el comando **exploit** para iniciar el ataque


Cuando aparezca el aviso que una shell se ha abierto, al ejecutar el comando **whoami**, se tendrá acceso root
![Exploit](img/exploit.png)



## 🧪 Post-Laboratorio
Una vez finalizada la máquina, en la terminal donde se tiene desplegada la máquina vulnerable se utilizará la combinación de teclas **Control + C** para eliminarla.

![Cerrar laboratorio](img/cerrar.png)

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
