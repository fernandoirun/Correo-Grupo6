# ASIR-Practica-Correo-FDLA
Repositorio de Fernando Irún, Daniel Estévez, Lucas Garcia Redruello, Álvaro Rodríguez para la implementación y configuración de un sistema de correo electrónico completo (MTA y MDA) sobre Ubuntu Server, utilizando Postfix para el envío mediante SMTP y Dovecot para el acceso a buzones mediante IMAP/POP3, integrando seguridad TLS y pruebas de cliente con Thunderbird


Este repositorio contiene la configuración necesaria para desplegar un MTA (Mail Transfer Agent) utilizando Postfix sobre contenedores Docker. El objetivo es proporcionar un servicio de envío de correos mediante el protocolo SMTP, integrado con seguridad TLS y preparado para trabajar conjuntamente con un MDA (Dovecot), cuya configuración se encuentra en la rama creada por fernando.

Para facilitar la portabilidad y el despliegue, el servicio se ha orquestado mediante Docker Compose.

El archivo define la imagen personalizada, el mapeo de puertos esenciales para SMTP y la persistencia de los datos.

<img width="545" height="264" alt="image" src="https://github.com/user-attachments/assets/88a661b7-5eec-40cd-9e7d-4f63e67f1137" />

El Dockerfile utiliza una base de Ubuntu 24.04 y automatiza la instalación de las dependencias necesarias.

Definición del Entorno (Dockerfile)

  - Base: Ubuntu 24.04.

  - Paquetes: Postfix, mailutils y módulos SASL para autenticación.

  - Automatización: Se utiliza postconf para inyectar el nombre de host directamente durante la construcción.

<img width="696" height="359" alt="image" src="https://github.com/user-attachments/assets/bb7d6651-4008-485c-b5ee-361befdbaf64" />

El archivo main.cf es el núcleo del funcionamiento del servidor de correo. Se han ajustado los parámetros de red y entrega local.

<img width="626" height="133" alt="image" src="https://github.com/user-attachments/assets/966c4dab-ac23-48aa-951f-0695ba3e494d" />

