
<center><h1>✍🏻 WriteUps & CTF Solutions</h1></center>  
<p align="center">

![Banner](img/banner.png)

</p>

Este repositorio contiene todos los **WriteUps y soluciones** de diferentes plataformas de ciberseguridad como **DockerLabs**, HTB, TryHackMe y otras.  
Los WriteUps están organizados por **plataforma y dificultad**, con explicaciones detalladas, screenshots y comandos utilizados en cada paso.
 
> [!IMPORTANT]
> 
> Este repositorio se actualiza constantemente con nuevos WriteUps y soluciones

### **Máquinas Documentadas: 18**

<br>

## ❓ ¿Qué son los WriteUps?

Un **WriteUp** es un documento que explica **paso a paso** cómo se resolvió un desafío de Un write-up en ciberseguridad es un informe detallado que explica cómo se ha identificado, explotado y resuelto una vulnerabilidad o reto de seguridad, como los que se encuentran en CTFs, pruebas de penetración o investigaciones de fallos en sistemas. Incluye el contexto del reto, el proceso de análisis, las técnicas y herramientas utilizadas, la explotación o resolución, y las lecciones aprendidas, normalmente con ejemplos, capturas y código para que otros puedan entenderlo.

Realizar write-ups tiene varias ventajas: permite aprender de forma profunda al obligarte a documentar cada paso; sirve como historial profesional y portafolio, mostrando tus habilidades técnicas; mejora la capacidad de comunicación al explicar conceptos complejos; funciona como referencia para futuros retos o proyectos reales; ayuda a construir reputación y networking si se comparte en comunidades o GitHub; y desarrolla el pensamiento crítico y la atención al detalle, fundamentales en ciberseguridad.
<br>

## 🐋 WriteUps en DockerLabs
![DockerLabs](dockerlabs.png)

Soluciones de máquinas de **DockerLabs** organizadas por dificultad.

### Muy Fácil 🔵

- [`Obsessions`](./DockerLabs/Obsessions/README.md) → Enumeración de servicios, análisis web y explotación de FTP, SSH y HTTP, con escalada de privilegios mediante abuso de sudo y binarios vulnerables.

- [`Vacaciones`](./DockerLabs/Vacaciones/README.md) → Enumeración de servicios, fuerza bruta por SSH, análisis de servidor HTTP y escalada de privilegios en Linux.

- [`tproot`](./DockerLabs/tproot/README.md) → Enumeración de servicios y detección de VSFTPD 2.3.4 con backdoor explotable.

- [`BreakMySSH`](./DockerLabs/breakmyssh/README.md) → Máquina orientada a la enumeración básica, fuerza bruta y cracking de hashes para obtener acceso inicial mediante credenciales descubiertas y acceso por SSH.

- [`Borazuwarahctf`](./DockerLabs/borazuwarahctf/README.md) → Máquina orientada al análisis de metadatos y fuerza bruta para obtener acceso inicial mediante credenciales descubiertas en un servicio web y acceso por SSH.

- [`Trust`](./DockerLabs/trust/README.md) → Enumeración web y fuerza bruta para obtener acceso inicial mediante credenciales descubiertas en un servicio web y acceso por SSH, con escalada de privilegios a través de binarios sudo inseguros.

- [`Hedgehog`](./DockerLabs/hedgehog/README.md) → Enumeración de servicios OpenSSH y HTTP para identificar credenciales débiles y obtener acceso inicial mediante fuerza bruta.

- [`FirstHacking`](/DockerLabs/firsthacking/README.md) → Explotación de servicio FTP vulnerable para obtener acceso inicial directo al sistema y privilegios root mediante Metasploit.
<br>

### Facil 🟢

- [`WalkingCMS`](/DockerLabs/walkingcms/README.md) → Explotación de un entorno WordPress mediante enumeración web, fuerza bruta de credenciales CMS, obtención de acceso remoto a través de edición de archivos del tema y escalada de privilegios mediante binarios SUID inseguros en Linux.
  
- [`Walking_Dead`](/DockerLabs/Walking_Dead/README.md) → Explotación de un servidor web en Apache HTTP Server mediante análisis de código fuente, ejecución remota de comandos a través de una web shell oculta, obtención de acceso remoto con reverse shell y escalada de privilegios mediante binarios SUID inseguros en Linux.

- [`AnonymousPingu`](/DockerLabs/AnonymousPingu/README.md) → Explotación de un servicio File Transfer Protocol con acceso anónimo y de un servidor web Apache HTTP Server mediante subida de archivos maliciosos, ejecución remota de código a través de una web shell en PHP, obtención de acceso remoto mediante reverse shell y escalada de privilegios en Linux mediante abuso de binarios sudo inseguros documentados en GTFOBins.

- [`Backend`](/DockerLabs/backend/README.md) → Explotación de una aplicación web vulnerable en entorno Linux mediante reconocimiento de servicios expuestos, análisis de formulario de autenticación, explotación de SQL Injection con sqlmap para obtener credenciales, acceso inicial por SSH y escalada de privilegios mediante abuso de binarios SUID inseguros y crackeo de hash MD5 con John the Ripper.

### Medio 🟡

- [`ChocolateFire`](/DockerLabs/chocolateFire/README.md) → Explotación de servicios mal configurados en entornos Linux mediante enumeración de servicios expuestos, acceso a paneles administrativos con credenciales débiles, obtención de acceso remoto al sistema a través de servicios vulnerables y escalada de privilegios mediante abuso de permisos sudo inseguros y configuraciones incorrectas documentadas en GTFOBins.
  
- [`Dance-Samba`](/DockerLabs/dance-samba/README.md) → Explotación de servicios mal configurados en entornos Linux mediante técnicas de reconocimiento y enumeración de servicios expuestos, identificación de credenciales débiles y acceso inicial al sistema a través de servicios vulnerables como FTP, SMB o SSH. Posteriormente, se realiza la explotación de configuraciones inseguras para obtener acceso remoto y se lleva a cabo la escalada de privilegios mediante el abuso de permisos sudo mal configurados y binarios explotables documentados en GTFOBins, permitiendo la toma completa del control del sistema.

- [`Domain`](/DockerLabs/domain/README.md) → Explotación de servicios mal configurados en un entorno Linux mediante técnicas de reconocimiento y enumeración de servicios expuestos como HTTP y SMB, identificación de usuarios válidos y obtención de credenciales mediante comprobaciones de acceso y fuerza bruta controlada sobre Samba. Posteriormente, se aprovecha el acceso a un recurso compartido para subir una reverse shell en PHP y obtener acceso inicial al sistema como usuario del servidor web. Finalmente, se realiza la escalada de privilegios mediante el abuso de un binario con permisos SUID, permitiendo modificar ficheros críticos del sistema y comprometer completamente la máquina como root.


### Dificil 🔴

- [`Vulnerame`](/DockerLabs/vulnerame/README.md) → Explotación de servicios mal configurados en entornos Linux mediante técnicas de reconocimiento y enumeración de servicios expuestos como HTTP, SSH y MySQL, identificación de información sensible en aplicaciones web (Joomla) y aprovechamiento de una vulnerabilidad de information disclosure para la obtención de credenciales. Posteriormente, se realiza el acceso a la base de datos para la extracción y cracking de hashes de usuarios, lo que permite el acceso inicial al panel de administración y la explotación de plantillas PHP para obtener una reverse shell y acceso al sistema. Finalmente, se lleva a cabo la escalada de privilegios mediante enumeración de usuarios locales, fuerza bruta contra el servicio SSH y el abuso de permisos sudo sobre un script en Ruby, permitiendo la modificación del mismo para ejecutar una shell y lograr la toma completa del control del sistema como root.

## 📦 WriteUps en HackTheBox
![HackTheBox](hackthebox.png)


Soluciones de máquinas de **HackTheBox** organizadas por dificultad.


### Fácil 🟢

- [`Cap`](./HackTheBox/Facil/Cap/README.md) → Explotación de un fallo IDOR en una aplicación web para acceder a capturas de red sensibles, obtener credenciales reutilizables y escalar privilegios mediante capacidades especiales en binarios del sistema Linux.


## 🔢 WriteUps en TryHackMe
![TryHackMe](tryhackme.png)

### 5 Minutos 🔵

- [`Neighbour`](TryHackme/5_minutos/Neighbour/README.md) → Identificación de credenciales expuestas en el código fuente y explotación de una vulnerabilidad IDOR mediante la manipulación de parámetros en la URL para acceder a información restringida de otro usuario.

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
