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

### Logs Docker

<img width="1528" height="136" alt="image" src="https://github.com/user-attachments/assets/28752634-41b9-4b3c-a181-d31b2da390a0" />


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

### IAC con Terraform

Instalamos el CLI de azure:

<img width="1265" height="1228" alt="image" src="https://github.com/user-attachments/assets/1966100c-7fd3-4ce9-9ff3-c9073d7877cf" />

Instalamos terraform con el siguiente comando:

<img width="695" height="39" alt="image" src="https://github.com/user-attachments/assets/080bac68-89db-4a95-ab75-79c76a7f101e" />

Debería funcionar, en este caso indica que ya está instalado dado a que fué instalado anteriormente :)

Nos logueamos con la CLI de azure en nuestra cuenta proporcionada por el centro:

<img width="1274" height="108" alt="image" src="https://github.com/user-attachments/assets/ceec8443-814f-4b2b-9393-82c2edcd5e5a" />

Para el despliegue con terraform, creamos dentro de una carpeta, llamada "terraform" en nuestro directorio home, un archivo de configuración llamado "main.tf" en el que configuraremos como corresponda, escribiendo nuestro grupo de recursos, y lo correspondiente, en el archivo de configuración, como se ve en la siguiente imagen:

<img width="1270" height="1239" alt="image" src="https://github.com/user-attachments/assets/39c3c339-9e5b-4e78-b57c-769986dddfd8" />

Lo arrancamos y comprobamos que arranca correctamente:

<img width="768" height="464" alt="image" src="https://github.com/user-attachments/assets/59e3732b-a680-40f8-b86c-667c62a18016" />

Lo arrancamos para comprobar su funcionamiento:

<img width="1242" height="1312" alt="image" src="https://github.com/user-attachments/assets/fff5de65-bda5-47b2-aafd-d93bcb0fb619" />

<img width="1240" height="894" alt="image" src="https://github.com/user-attachments/assets/e0210d2e-bc76-45e1-b4ee-2681cd50fbba" />

