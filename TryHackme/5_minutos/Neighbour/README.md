<center><h1>Neighbour</h1></center>  
<p align="center">

<img src="img/banner.png" width="400">


## ❓ ¿Qué es Neighbour?

Neighbour es una máquina web sencilla enfocada en la identificación de credenciales expuestas en el código fuente y la explotación de una vulnerabilidad **IDOR (Insecure Direct Object Reference)**. Permite practicar análisis básico de aplicaciones web, manipulación de parámetros en URL y comprensión de fallos de control de acceso en aplicaciones vulnerables.


## 🔝 Despliegue Neighbour

Tras descargar el archivo ovpn que ofrece [la plataforma](https://tryhackme.com/access), es necesario en terminal ejecutar el comando **sudo openvpn archivo vpn**


![Conexión VPN](img/conexion_vpn.png)

Además, es necesario comprobar conectividad entre nuestra máquina kali y el objetivo .

![Conectividad](img/conectividad.png)

> [!NOTE]
>
> La dirección IP mostrada en la siguiente imagen es solo de referencia, ya que TryHackMe asigna una IP diferente en cada despliegue de la máquina.

## 🔍 Solución

Al entrar por primera vez a esta página web, se encuentra un panel de logueo, el cúal las credenciales se extraen a través del código fuente presionando **Control + U**

![Iniciar Sesión](img/log_in.png)

Dentro del código fuente del panel de logeo se encuentra un comentario en la línea 34 con el usuario **guest** y contraseña **guest**.

![Codigo fuente](img/codigo_fuente.png)

Tras iniciar sesión, se observa un mensaje de bienvenida al usuario autenticado y una URL que contiene el parámetro `?user=guest`. Esto sugiere una posible vulnerabilidad **IDOR (Insecure Direct Object Reference)**, ya que el identificador del usuario se controla directamente mediante un parámetro modificable en la URL.

Dado que en el código fuente aparecía una referencia al usuario **admin**, indicando además ciertas limitaciones asociadas a dicho perfil, se deduce que este usuario existe dentro de la aplicación. Por ello, se modifica manualmente el valor del parámetro, sustituyendo `guest` por `admin`, con el objetivo de comprobar si es posible acceder a recursos pertenecientes a otro usuario.

![web](img/web.png)

Ya cambiado, se encuentra la flag del usuario admin.

![Flag](img/acceso_admin.png)

Se verifica la flag en la tarea Neighbour.
![Task](img/task.png)


## 🔧 Remediación

Para evitar una vulnerabilidad IDOR, el servidor debe comprobar que cada usuario solo pueda acceder a sus propios datos, aunque se modifique el parámetro de la URL.

En este caso, aunque se cambie `guest` por `admin`, la aplicación debería verificar si ese acceso está autorizado antes de mostrar la información. Además, evitar utilzar parámetros críticos en las URLs.

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
