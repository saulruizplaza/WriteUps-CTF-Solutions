<center><h1> Obsession</h1></center>  
<p align="center">

![Banner](img/banner.png)


## ❓ ¿Qué es Obsession?

Obsession es una máquina vulnerable orientada al aprendizaje de ciberseguridad ofensiva, centrada en la enumeración de servicios, análisis web y escalada de privilegios en sistemas Linux. A través de servicios mal configurados como FTP, SSH y HTTP, el usuario deberá recopilar información clave para obtener acceso al sistema y, posteriormente, elevar privilegios hasta root mediante el abuso de permisos sudo y binarios vulnerables.


Esta máquina está pensada para quienes quieren dar sus primeros pasos en pentesting y comprender la importancia de una correcta configuración de servicios y permisos en entornos Linux.

> [!NOTE]
>
>Puede descargar la máquina a través del **[enlace mega](https://mega.nz/file/JHUEFZ4J#SyfKRfM6_xKBXLxP8JZKW-sVQnB0Nv2B1Dwbw6pRn9w)**


## 📹 ¡Mira el procedimiento en mi canal de YouTube!

|  |
|---|
| [![Obsession - Tutorial Práctico](https://img.youtube.com/vi/a5V7oiIcbYw/maxresdefault.jpg)](https://www.youtube.com/watch?v=a5V7oiIcbYw) |



## 🔝 Despliegue Obsession

Al descargar la máquina, es necesario descompromirlo para poder encontrar los archivos necesarios para poder desplegarla, para ello, utilizaremos el comando.

**unzip obsession.zip.**

![unzip obsession.zip](img/unzip.png)

Obtendremos dos ficheros:
- **Auto_deploy.sh:** Script Bash para desplegar nuestra máquina localmente.
- **Obsession.tar:** Máquina vulnerable contenizada.

Para desplegar el servicio será necesario carle permisos de ejecución a auto_deploy.sh, ya que por defecto tiene permisos 644. Para ello, usaremos el comando:

 **chmod +x auto_deploy.sh**

 Una vez ejecutado, se utilizará el comando **./auto_deploy.sh obsession.tar** para lanzar la máquina

![Despliegue](img/despliegue.png)


## 🔎 Fase de Descubrimiento 
Ahora, se abrirá una nueva terminal para empezar a realizar el descubrimiento del sistema. Cómo sabemos la dirección IP de la máquina vulnerable **(172.17.0.2)**, comenzaremos realizando un escaneo de red nmap. 
En esta ocación, se usará el comando **nmap -sC -sV --min-rate 5000 172.12.0.2**


| Argumento | Significado |
|---|---|
| -sC | Ejecuta los scripts para comprobaciones comunes |
| -sV | Detección de versiones de servicios |
| --min-rate 5000 | Envía al  5000 paquetes por segundo (aumenta velocidad; puede causar pérdida o detección) |
| 172.17.0.2 | Dirección IP del objetivo a escanear |


![Escaneo de Red](img/escaneo.png)

> [!NOTE]
>
>Se ha realizado un escaneo agresivo debido a que se está realizando en un entorno controlado y no es importante el ser detectado. Si se busca hacer el mínimo ruido posible será necesario utilizar el argumento **-sS** se usa para no ser detectado fácilmente, porque no completa la conexión TCP. Además, **no se usará --min-rate.**



Cómo vemos, se encuentra tres servicios activos:
- **FTP con acceso anónimo (Puerto 21):** Se encuentra 2 ficheros.txt
- **SSH (Puerto: 22):** para conexiones remotas: Será el servicio que usaremos para entrar al equipo.
- **HTTP (Puerto 80):** Servidor Web.


## 📂 Análisis FTP
Primero se intenta acceder al servidor FTP de forma anónima, ya que normalmente viene configurado por defecto con las credenciales **anonymous / anonymous**. Si esto no funciona, se puede probar fuerza bruta con Hydra o utilizar scripts de Nmap para enumerar el servicio.

Para acceder al servidor FTP se usa el comando **ftp**. Posteriormente se utiliza el comando **open 172.17.0.2** para abrir la conexión con el servidor. Por último, se introduce las credenciales.

![Acceso ftp](img/ftp.png)

Con el comando **dir** se podrá listar los archivos disponibles.
![Listar FTP](img/listar_ftp.png)

A continuación, se descarga los ficheros utilizando el comando **get**

![Descarga de ficheros](img/descarga_ficheros.png)
Una vez descargado, con **cat**, se verá el contenido de estos ficheros.



![Chat gonza](img/chat_gonza.png)
<center><b>Chat-gonza.txt</b></center>

En el segundo documento, se puede encontrar información importante en el cuarto punto: Dice que algunos permisos habilitados cree que no son del todo seguros. Una pista para cuando se escale privilegios.
![pendientes](img/pendientes.png)
<center><b>pendientes</b></center>

## 🌐 Análisis Web
A continuación, se empezará a analizar el servidor web.

![Web](img/web.png)

Se encuentra un formulario web, donde redirige a esta página.
![Output_form](img/output_form.png)

Nos interesa la raíz de la web, donde se verá el código fuente del servidor usando **Control + U**.

En la **línea 52**, se encuentra una pista, donde índica que se utiliza el mismo usuario para todos los permisos.

![Codigo Fuente](img/codigo_fuente.png)

Ahora, cómo no se cuenta con más directorio faciles de encontrar, usaremos dirbuster para realizar una enumeración de directorios:

Será necesario establecer la dirección IP del servidor objetivo, con su protocolo (http) y puerto (:80). Posteriormente se establecerá el número de hilos (velocidad), en mi caso, he usado 200 que es el máximo para mí. Por último, se ha establecido un diccionario de directorios comunes para que dirbuster haga peticiones y nos reporte el estado de cada uno, estos diccionarios se encuentran en **/usr/share/wordlist**.

![Configuración Dirbuster](img/dirbuster.png)

Una vez terminada la prueba, se mostrará el resultado en el apartado **Results - List View**.

![Resultado Dirbuster](img/resultado_dirbuster.png)

A continuación, se muestra la información extraída mediante los directorios y archivos encontrados con respuesta 200 (OK):

- **/backup/backup.txt:** Usuario para los servicios: russoski.
  ![Backup](img/backup.png)

- **/important/important.md:** Manifiesto Hacker. No hay información relevante .
  ![important.md](img/important_md.png)


## 🛜 Análisis SSH
Cómo ya conocemos el usuario, podemos realizar un ataque de fuerza bruta para encontrar la contraseña. Se puede realizar de dos maneras:
- **Nmap:** nmap -p 22 --script ssh-brute --script-args userdb=users.txt --min-rate 5000 172.17.0.2


    | Argumento | Significado |
    |---|---|
    | -p 22 | Especifica el puerto SSH a escanear. |
    | --script ssh-brute | Ejecuta el script NSE para ataque de fuerza bruta en SSH. |
    | --script-args userdb=users.txt | Proporciona el archivo con lista de usuarios para el ataque. |
    | --min-rate 5000 | Envía al menos 5000 paquetes por segundo (aumenta velocidad). |

>[!CAUTION]
>
>Se debe crear con un editor de texto cómo **nano**  llamado **users.txt** con el usuario russoski dentro.


  ![Nmap Fuerza Bruta](img/nmap_fbruta.png)

- **Hydra [RECOMENDADO]**: hydra -l ryssoski -P /usr/share/wordlists/rockyou.txt.gz ssh://172.17.0.2 -t 64


    | Argumento | Significado |
    |---|---|
    | hydra | Herramienta de ataque de fuerza bruta. |
    | -l russoski | Especifica un usuario. |
    | -P /usr/share/wordlists/rockyou.txt.gz| Archivo con diccionario de contraseñas a probar. |
    | ssh://172.17.0.2| Protocolo y dirección IP del objetivo. |
    | -t 64 | Número de hilos utilizados (velocidad). |

    ![Hydra](img/hydra.png)

## 🖥️ Acceso al servidor
Cómo ya se ha obtenido el usuario (russoski) y su contraseña (iloveme), se procede a la conexión por ssh mediante el comando **ssh russoski:172.17.0.2**.

![Acceso SSH](img/acceso_ssh.png)

Se lista todos los recursos que tenemos disponible:

![Listar](img/listar.png)

## 🔓 Escalada de permisos
Se usará el comando **sudo -l** para mostrar los binarios que el usuario russoski puede ejecutar cómo **root**.

![binarios sudo](img/binarios_sudo.png)

A continuación, se consulta **[GTFOBins](https://int0x33.github.io/)** para identificar posibles técnicas de escalada de privilegios, ya que el binario vim puede ejecutarse con permisos elevados. En esta plataforma se revisa qué acciones pueden realizarse con dicho binario para obtener una shell o ejecutar comandos como usuario root.

![GTFObins](img/gtfobins.png)

Posteriormente se introduce el comando, se confirma que se accede al usuario root usando el comando **whoami**.

![Root](img/root.png)



## 🧪 Post-Laboratorio
Una vez finalizada la máquina, en la terminal donde se tiene desplegada la máquina vulnerable se utilizará la combinación de teclas **Control + C** para eliminarla.

![Cerrar laboratorio](img/cerrar.png)


## <img src="./img/logoactual.jpg" width=25 style="; border-radius:50%;">  ¡Hola! Me llamo Saúl Ruiz 
### Analista de Ciberseguridad | Seguridad Ofensiva y Pentesting


![YouTube](https://img.shields.io/youtube/channel/subscribers/UCcOkvgreZrXauRHyXlii0JA)
![Seguidores](https://img.shields.io/github/followers/saulruizplaza)
[![Twitter Follow](https://img.shields.io/twitter/follow/plasysx?style=social)](https://twitter.com/plasysx)


Soy Analista de Ciberseguridad y Técnico Superior en Administración de Sistemas Informáticos en Red. Actualmente desarrollo mi carrera en entornos SOC, participando en tareas de análisis, monitorización e investigación de eventos de seguridad.


Mi interés principal se orienta hacia la seguridad ofensiva, el pentesting y el análisis técnico, áreas en las que sigo formándome de manera constante para crecer profesionalmente dentro del sector.

A través de mi proyecto personal <b>[@PlaSysX](https://linktr.ee/PlaSysx)</b>, comparto contenido relacionado con informática, ciberseguridad y aprendizaje práctico, con el objetivo de aportar valor a quienes también quieren seguir creciendo en el mundo tecnológico.


[![Website](https://img.shields.io/badge/Website-plasysx.com-7B3FF2?style=for-the-badge&logo=google-chrome&logoColor=white&labelColor=101010)](https://plasysx.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Saúl_Ruiz_Plaza-0077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=101010)](https://www.linkedin.com/in/saulruizplaza) [![Instagram](https://img.shields.io/badge/Instagram-@PlaSysX-E4405F?style=for-the-badge&logo=instagram&logoColor=white&labelColor=101010)](https://instagram.com/plasysx)
[![TikTok](https://img.shields.io/badge/TikTok-@plasysx_es-69C9D0?style=for-the-badge&logo=tiktok&logoColor=white&labelColor=101010)](https://tiktok.com/@plasysx_es)
[![YouTube](https://img.shields.io/badge/YouTube-Plasysx-FF0000?style=for-the-badge&logo=youtube&logoColor=white&labelColor=101010)](https://youtube.com/@Plasysx)
[![Twitter](https://img.shields.io/badge/Twitter-@plasysx-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white&labelColor=101010)](https://twitter.com/plasysx)
