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

Para poner en marcha el sistema, EJECUTAMOS EL SIGUIENTE COMANDO:

<img width="1269" height="267" alt="image" src="https://github.com/user-attachments/assets/e414e5cd-afde-4cfa-a6ec-3fdd56bdb6e7" />

Y con el siguiente verificamos que está a escucha:

<img width="1260" height="113" alt="image" src="https://github.com/user-attachments/assets/773532c3-ba3e-486a-ae6b-456751bdc367" />

Esto, lo pasaríamos a local, y con un cliente o un webmail, podríamos desplegarlo y todo, y comprobar su funcionamiento, en la nube como acabamos de desplegarlo no, dado que el dominio al que apunta, no existe, lo tenemos en nuestras máquinas virtuales en local.

## Demo

Clonamos repositorios para instalar y activar los contenedores en la máquina servidor:

<img width="932" height="644" alt="image" src="https://github.com/user-attachments/assets/629138d6-4cc8-4569-8100-05cf737f596a" />

Instalamos también la configuración de dovecot y modificamos el dominio de mi compañero por el mío:

<img width="1879" height="902" alt="image" src="https://github.com/user-attachments/assets/e8c2c140-703b-4320-8ed0-bfa253493112" />

Instalamos thunderbird:

<img width="1262" height="311" alt="image" src="https://github.com/user-attachments/assets/e1cd75e6-c110-4fcc-8ba8-fdef5035d78d" />

<img width="1884" height="143" alt="image" src="https://github.com/user-attachments/assets/a8eb1abe-6fbf-4513-b9da-fe8cb8a63c97" />

<img width="1873" height="178" alt="image" src="https://github.com/user-attachments/assets/064e15d3-fd6b-4040-a83a-697f15b194f8" />

Podemos ver que ambos servicios, están funcionando.

