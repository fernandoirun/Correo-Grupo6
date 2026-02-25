# Daniel Estévez - Parte 1 -> Postfix
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

## Demo en local

Clonamos repositorios para instalar y activar los contenedores en la máquina servidor:

<img width="932" height="644" alt="image" src="https://github.com/user-attachments/assets/629138d6-4cc8-4569-8100-05cf737f596a" />

Instalamos también la configuración de dovecot y modificamos el dominio de mi compañero por el mío:

<img width="1879" height="902" alt="image" src="https://github.com/user-attachments/assets/e8c2c140-703b-4320-8ed0-bfa253493112" />

Instalamos thunderbird:

<img width="1262" height="311" alt="image" src="https://github.com/user-attachments/assets/e1cd75e6-c110-4fcc-8ba8-fdef5035d78d" />

<img width="1919" height="200" alt="image" src="https://github.com/user-attachments/assets/ce9a299a-42ed-49fc-9179-ef1774a8ca06" />

<img width="1873" height="178" alt="image" src="https://github.com/user-attachments/assets/064e15d3-fd6b-4040-a83a-697f15b194f8" />

Podemos ver que ambos servicios, están funcionando, ambos configurados con el dominio correspondiente de la red local, para que pueda resolver.

También podemos hacerlo sin necesidad de contenedores, como veremos a continuación:

Instalamos postfix:

<img width="895" height="345" alt="image" src="https://github.com/user-attachments/assets/88c37828-eb2d-4585-903d-0142cfdb5b98" />

Elegimos Internet Site:

<img width="852" height="744" alt="image" src="https://github.com/user-attachments/assets/12f0a508-e919-4a6e-9363-1d27f338ffcc" />

Ponemos de nombre del host el FQDN del dominio como indica la práctica y la info que nos aparece en pantalla:

<img width="856" height="456" alt="image" src="https://github.com/user-attachments/assets/406e03ec-5b4e-4755-b6d7-18d2688301f8" />

Seleccionamos correo para el administrador del dominio:

<img width="841" height="510" alt="image" src="https://github.com/user-attachments/assets/be24bcd7-4d2c-4cb2-a4c0-4895ded45029" />

No forzamos sincronizacion de correos:

<img width="823" height="268" alt="image" src="https://github.com/user-attachments/assets/c38054da-8a81-43b3-93c8-b926d276641e" />

Limitamos correos, su tamaño, a 20MB, tras poner nuestros rangos de red:

<img width="850" height="304" alt="image" src="https://github.com/user-attachments/assets/f4dfa1f3-127c-4973-b70d-11a0d5fc6592" />

Y seleccionamos ipv4 como unico protocolo de red permitido, denegando así cualquier petición por ipv6:

<img width="836" height="453" alt="image" src="https://github.com/user-attachments/assets/ddcf6e01-a915-47ab-b9cb-f672fce9e1e9" />

Comprobamos configuración de Postfix:

<img width="669" height="600" alt="image" src="https://github.com/user-attachments/assets/9d328306-8f91-4a5f-b6bf-8ea2f3555685" />

Y las directivas puestas por nosotros mismos:

<img width="914" height="624" alt="image" src="https://github.com/user-attachments/assets/fd4574f6-7ad2-4b46-a97a-94fe8d7861b0" />

<img width="892" height="54" alt="image" src="https://github.com/user-attachments/assets/c3701bbd-701c-4cb3-9be7-bb9d7984446e" />

<img width="901" height="690" alt="image" src="https://github.com/user-attachments/assets/482fb211-8364-4f7a-aa7a-0370b555a629" />

Tras configurar postfix, instalamos certbot, para hacer los certificados y configurar el SMTP seguro, con certificados no autofirmados:

<img width="680" height="155" alt="image" src="https://github.com/user-attachments/assets/8db69d10-ba93-4cd9-ad2f-b7bfe71d5f04" />

Lo configuramos:

<img width="1775" height="750" alt="image" src="https://github.com/user-attachments/assets/f10b56a3-ff2e-4ced-8f8f-75bc90e9d4f1" />

Da error, ya que Certbot requiere que el dominio esté registrado públicamente en internet, por lo que da error, ya que no somos propietario del dominio, ni tenemos IP Pública en la red privada, ya que esta la tiene el router. Por lo que seguiremos usando ssl, pero con certificados autofirmados, para ello, creamos la siguiente estructura de directorios:

<img width="697" height="45" alt="image" src="https://github.com/user-attachments/assets/9bb35cbc-9d67-483f-a5ab-25130270581f" />

Creamos el certificado autofirmado con lo siguiente:

<img width="908" height="120" alt="image" src="https://github.com/user-attachments/assets/2086ca39-6757-4372-be87-1386e9ba56c8" />

<img width="697" height="837" alt="image" src="https://github.com/user-attachments/assets/8065c68f-25cd-4ce8-87bd-8044e472dedc" />

Ya está creado!

Modificamos los permisos ya que los certificados deben tener x permisos para que otros usuarios del sistema no puedan leerlos ni usarlos:

<img width="899" height="75" alt="image" src="https://github.com/user-attachments/assets/da9f53ee-0936-4b13-b239-76563390dbe5" />

Ponemos los certificados autofirmados en el archivo main.cf:

<img width="621" height="97" alt="image" src="https://github.com/user-attachments/assets/04576f4a-dadd-4b37-adcc-6f69ca155f34" />

Comprobamos abriendo una conexión con telnet al puerto 25 (SMTP), que soporte TLS:

<img width="729" height="405" alt="image" src="https://github.com/user-attachments/assets/17940b9c-54a8-4050-970e-49ab97989f54" />

<img width="880" height="354" alt="image" src="https://github.com/user-attachments/assets/7d1e44c0-0067-46bd-9ec3-c7fb2be8c936" />

Revisamos los logs:

<img width="891" height="377" alt="image" src="https://github.com/user-attachments/assets/e9f994dd-9f5d-4bf8-b12f-543f58bab889" />

<img width="704" height="250" alt="image" src="https://github.com/user-attachments/assets/e390330a-97b9-4b74-a659-ac1f4e4ffab9" />

Procedemos a la instalación de Dovecot:

<img width="879" height="85" alt="image" src="https://github.com/user-attachments/assets/4f74b235-95eb-4051-aac6-6b74d85a7dff" />

Comprobamos que usamos mbox como formato de fichero para el buzón:

<img width="604" height="40" alt="image" src="https://github.com/user-attachments/assets/a03a17d1-f2b1-47c3-ae34-6db49ba842b5" />

Comprobamos la configuración efectiva del servidor:

<img width="617" height="795" alt="image" src="https://github.com/user-attachments/assets/8831f2fa-0b51-4530-9ee5-074cef047af8" />

Añadimos la siguiente configuración en el archivo 10-auth.conf para que permita la autenticación insegura con la passw en texto plano:

<img width="736" height="753" alt="image" src="https://github.com/user-attachments/assets/157e3ad4-1e50-488b-a0b0-0d85f7e187a2" />

Copiamos el certificado ssl autofirmado de postfix para usarlo con dovecot y le damos los permisos necesarios:

<img width="887" height="141" alt="image" src="https://github.com/user-attachments/assets/c9a3ff2e-bbe4-4473-a454-a31af14c1d0c" />

Ponemos estas configuraciones en el archivo ssl de dovecot para el uso de los certificados SSL y protocolos seguros:

<img width="684" height="324" alt="image" src="https://github.com/user-attachments/assets/3b5a5278-ae7d-4c03-9f4e-bd21c314381a" />

Instalamos MailUtils:

<img width="750" height="97" alt="image" src="https://github.com/user-attachments/assets/46953534-3064-4113-8360-b07587bca8e2" />

Comprobamos que podamos mandar correos con el cliente mail:

<img width="936" height="1036" alt="image" src="https://github.com/user-attachments/assets/0be52a7e-9121-4902-8961-f9de73aa7031" />

<img width="752" height="202" alt="image" src="https://github.com/user-attachments/assets/973b659b-23f8-481c-a11e-32432101d22c" />

Podemos ver como enviamos con root un correo al usuario adm_destevez y le llega.

Podemos abrirlo:

<img width="786" height="390" alt="image" src="https://github.com/user-attachments/assets/4cb6a79b-8f7f-484c-b131-f34b2d1dae84" />

Probamos con thunderbird:

<img width="757" height="583" alt="image" src="https://github.com/user-attachments/assets/c6ecd820-c9d7-46ce-8991-a81bff083bdf" />

<img width="599" height="943" alt="image" src="https://github.com/user-attachments/assets/993c8b8a-7589-4d1f-934a-c1414bad2100" />

Comprobamos que la configuración funcine desde thunderbird con los certificados SSL:

<img width="594" height="966" alt="image" src="https://github.com/user-attachments/assets/26dd10c5-17fa-40e7-8163-f0b766a7b290" />

<img width="702" height="749" alt="image" src="https://github.com/user-attachments/assets/ab438b45-9553-44f7-a291-c6a1851dff16" />

<img width="1855" height="644" alt="image" src="https://github.com/user-attachments/assets/25b7d794-37d8-4272-961a-f7f7b18f1d42" />

Vemos al loguearnos con adm_destevez@destevez.fpinfo.com.es que vemos el mensaje que recibimos anteriormente de root, enviemos uno a root desde thunderbird, y lo vemos desde mail, y posteriormente, mandamos uno desde mail con root a adm_destevez a ver si llega y lo vemos en thunderbird:

<img width="1465" height="592" alt="image" src="https://github.com/user-attachments/assets/6d9877e1-9d05-4064-9699-7ffa4073f783" />

<img width="1072" height="345" alt="image" src="https://github.com/user-attachments/assets/5f6a9b22-031c-4bd6-a553-b9b83cbbe195" />

Enviado!

<img width="1099" height="590" alt="image" src="https://github.com/user-attachments/assets/90bfde7f-584a-4d40-9315-9fb2a7edb094" />

Recibido!

Enviemos ahora con root a adm_destevez:

<img width="1240" height="789" alt="image" src="https://github.com/user-attachments/assets/cef1756d-1672-4824-bc54-90cfb7678276" />

Podemos ver como llega la notificación también al enviarlo desde mail.

<img width="1022" height="235" alt="image" src="https://github.com/user-attachments/assets/6aa69bfc-4985-42e1-a7ee-b6e2e2467e2c" />

<img width="1842" height="394" alt="image" src="https://github.com/user-attachments/assets/b4497d0f-ee99-4c07-8fe4-4de36f5f3849" />

Vemos como perfectamente tenemos el mensaje desde thunderbird, lo que dice que está perfectamente conectado con el servidor postfix y dovecot.
# Documentación de Fernando Irún - Parte 2 Dovecot

## Configuración realizada
- Rama: fernandoirun/dovecot-config
- Dominio: feri.fpinto.com.es
- Servicio: Dovecot (IMAP/POP3)
- Formato buzones: Maildir
- Autenticación: archivo users

## Archivos importantes
- `dovecot/Dockerfile`
- `dovecot/conf/dovecot.conf`
- `dovecot/conf/users`
- `compose/docker-compose-dovecot.yml`

## Comandos utilizados

https://github.com/fernandoirun/Correo-Grupo6/blob/fernandoirun/dovecot-config/documentacion-Ferir/Comandos.md

## 👤 Resumen de Trabajo: Fernando Irún

Este documento detalla las contribuciones y responsabilidades que he asumido durante el desarrollo del proyecto de servicios de red.

---

### 1. Gestión de Infraestructura y Repositorio
Como responsable de la organización inicial, he ejecutado las siguientes tareas:
* **Despliegue del Repositorio:** Creación y configuración inicial del repositorio `Correo-Grupo6`.
* **Flujo de Trabajo Git:** Configuración de la autenticación mediante GitHub CLI (`gh auth login`) y gestión de permisos para los colaboradores.
* **Arquitectura de Directorios:** Diseño de la estructura base del proyecto para separar configuraciones, datos y documentación.

### 2. Implementación Técnica: Servidor Dovecot (IMAP/POP3)
Me he encargado de la implementación completa del servicio de acceso a correo bajo la rama `fernandoirun/dovecot-config`:

* **Contenerización (Dockerfile):**
    * Imagen base: **Ubuntu 24.04**.
    * Gestión de usuarios: Configuración de usuario `vmail` (buzones virtuales) y usuario `postfix` (integración SASL).
    * Estructura de almacenamiento: Implementación del formato **Maildir**.
* **Configuración del Servicio (`dovecot.conf`):**
    * Dominio configurado: `feri.fpinto.com.es`.
    * Soporte de protocolos: **IMAP, POP3 y LMTP**.
* **Gestión de Usuarios Virtuales:**
    * Creación de archivo de contraseñas con las cuentas: `test`, `admin`, `user1` y `user2`.
* **Modularización (`conf.d/`):**
    * Ajustes de seguridad en `10-auth.conf`.
    * Definición de rutas en `10-mail.conf`.
    * Exposición de puertos (143, 993, 110, 995) en `10-master.conf`.
* **Orquestación:** Configuración del servicio en `docker-compose.yml` con persistencia de volúmenes y preparación del socket de autenticación para Postfix.

### 3. Documentación y Evidencias
He generado un paquete de documentación detallada en la carpeta `documentacion-Ferir/`:
* **Guía Técnica:** README descriptivo con los pasos de instalación.
* **Evidencias:** Capturas de pantalla de la estructura del proyecto, archivos de configuración críticos, Dockerfile y el historial de ramas en Git.

---

### Resumen para Evaluación

> **Declaración de contribución:**
> "Como integrante del Grupo 6, he liderado la **creación y organización del repositorio central** en GitHub. Mi desarrollo técnico principal ha sido el **servidor Dovecot**: desde la construcción del Dockerfile personalizado y la lógica de usuarios virtuales, hasta la configuración avanzada de Maildir y la integración con Docker Compose. Toda mi labor está documentada y respaldada por commits periódicos en mi rama de trabajo. La fase de integración del socket de autenticación con Postfix y las pruebas finales de envío/recepción quedan delegadas para la siguiente fase del equipo."

## Demostraciones
Mostrar estructura de directorios creada

<img width="392" height="332" alt="image" src="https://github.com/user-attachments/assets/d108f209-33c8-4e0c-a538-8618e5057a9e" />

## Mostrar contenido del Dockerfile

<img width="393" height="352" alt="image" src="https://github.com/user-attachments/assets/4ddaca12-c71f-4a8a-8d54-97bb63af9dfa" />

En esta captura se muestra el contenido del `Dockerfile` que he creado para construir la imagen de **Dovecot**. Está basado en `ubuntu:24.04` como pide la práctica. Lo que hace es:

* **Instalar los paquetes necesarios** de Dovecot (core, imap, pop3, lmtpd).
* **Crear el usuario `vmail**` con UID 5000, que será el dueño de los buzones de correo.
* **Crear el usuario `postfix**` para que en el futuro pueda usarse el socket de autenticación.
* **Crear la estructura de directorios:** `/var/spool/postfix/private` para el socket y `/var/mail/vhosts/feri.fpinto.com.es` para los buzones en formato Maildir.
* **Exponer los puertos** 143 (IMAP), 993 (IMAPS), 110 (POP3) y 995 (POP3S).
* **Finalmente, ejecutar Dovecot** en primer plano con `dovecot -F`.

Este `Dockerfile` es personalizado porque usamos una imagen base de Ubuntu en lugar de una imagen oficial de Dovecot, lo que nos da más control sobre la configuración.

## Mostrar configuración principal de Dovecot

<img width="269" height="483" alt="image" src="https://github.com/user-attachments/assets/894f6bf7-b1cc-49a5-8a63-fe6518da9217" />

Esta captura muestra el archivo principal de configuración de **Dovecot**. Aquí definimos los aspectos más importantes del servidor de correo:

* **Dominio:** El dominio que usamos es `feri.fpinto.com.es`, que es el asignado a nuestro grupo.
* **Protocolos:** Soportamos los protocolos **IMAP**, **POP3** y **LMTP** (este último para la entrega local de correos).
* **Formato de buzones:** El formato es **Maildir**, que guarda cada correo en un archivo individual dentro de `/var/mail/vhosts/dominio/usuario`. Esto es más robusto que el formato `mbox`.
* **Permisos:** El usuario `vmail` (UID 5000) es el propietario de los buzones.
* **Carpetas por defecto:** Se definen **Drafts**, **Junk**, **Trash** y **Sent** para que los clientes de correo las reconozcan automáticamente.
* **Socket de autenticación:** La sección `service auth` está preparada para crear un socket en `/var/spool/postfix/private/auth`, que permitirá a Postfix validar usuarios.
* **Autenticación:** Los usuarios se gestionan mediante el archivo `/etc/dovecot/users` usando el driver `passwd-file`.
* **Modularidad:** Se incluyen las configuraciones adicionales de la carpeta `conf.d/`.

Este archivo es el corazón de Dovecot y coordina todo el funcionamiento del servidor.

## Mostrar archivo de usuarios

<img width="580" height="93" alt="image" src="https://github.com/user-attachments/assets/de87a440-5eae-44ba-933a-d356d7868bae" />

En esta captura se muestra el archivo `users` donde hemos definido las cuentas de correo para las pruebas del servidor **Dovecot**. El formato utilizado es el estándar para archivos `passwd`:
`usuario:{PLAIN}contraseña:UID:GID:gecos:home`.

Hemos creado **cuatro usuarios** de prueba:

* **test@feri.fpinto.com.es** (password: `grupo6pass`) → Usuario principal para pruebas genéricas.
* **admin@feri.fpinto.com.es** (password: `admpass`) → Pensado para tareas administrativas.
* **user1@feri.fpinto.com.es** (password: `pass123`) → Usuario de prueba adicional.
* **user2@feri.fpinto.com.es** (password: `pass456`) → Otro usuario de prueba.

### **Detalles de configuración:**

* **Identidad:** Todos los usuarios comparten el mismo **UID y GID (5000)**, que corresponde al usuario `vmail` creado en el `Dockerfile`.
* **Ruta Home:** La ruta de cada uno apunta a su buzón individual dentro de `/var/mail/vhosts/feri.fpinto.com.es/`, siguiendo la estructura **Maildir** configurada.
* **Simplicidad:** Este archivo permite que Dovecot autentique a los usuarios sin necesidad de bases de datos externas, lo cual es ideal para nuestro entorno de prácticas.

## Mostrar configuraciones adicionales

<img width="447" height="447" alt="image" src="https://github.com/user-attachments/assets/ec6b20de-6f5c-46ca-985f-a5a176296562" />

Este archivo sirve para **definir cómo se autentican los usuarios**. Hemos configurado el sistema para que utilice el archivo `users` como base de datos local de cuentas.

### **`10-mail.conf`**

Este archivo sirve para **indicar dónde y cómo se guardan los correos**. En nuestra configuración, hemos establecido el formato **Maildir** y la ruta jerárquica `/var/mail/vhosts/dominio/usuario`.

### **`10-master.conf`**

Este archivo sirve para **configurar los puertos y servicios** que ofrece Dovecot:

* **Puertos:** 143 (IMAP), 993 (IMAPS), 110 (POP3) y 995 (POP3S).
* **Socket de autenticación:** Prepara el socket en la ruta compartida para que **Postfix pueda validar usuarios** de forma externa cuando se implemente el envío.

 ## Mostrar docker-compose para Dovecot

 <img width="485" height="258" alt="image" src="https://github.com/user-attachments/assets/c72e86db-b2b5-4354-828b-90bde7095398" />

Este archivo de **Docker Compose** sirve para **orquestar el contenedor de Dovecot** de forma automatizada. A continuación, se detalla cada directiva utilizada:

* **`build: ../dovecot`**: Construye la imagen utilizando nuestro `Dockerfile` personalizado.
* **`container_name: dovecot-grupo6`**: Asigna un nombre específico al contenedor para identificarlo fácilmente en el sistema.
* **`hostname: mail.feri.fpinto.com.es`**: Define el nombre de host del servidor de correo dentro de la red.
* **`ports` (Mapeo de puertos)**: Expone los servicios al exterior mapeando los puertos del contenedor con los del host:
* **143** (IMAP) / **993** (IMAPS)
* **110** (POP3) / **995** (POP3S)


* **`volumes` (Persistencia y configuración)**: Monta carpetas y archivos locales dentro del contenedor para que los cambios sean permanentes:
* `../maildata`: Almacena los correos electrónicos (formato Maildir).
* `dovecot.conf`, `conf.d/` y `users`: Inyecta la configuración que hemos definido.
* `../postfix/spool`: Prepara el volumen para el socket compartido que usará Postfix en el futuro.


* **`restart: unless-stopped`**: Garantiza que el servicio se reinicie automáticamente si ocurre un error, a menos que lo detengamos manualmente.

Este archivo nos permite desplegar todo el servicio de correo con un solo comando: `docker compose up -d`.

## Construir y levantar el contenedor

<img width="957" height="360" alt="image" src="https://github.com/user-attachments/assets/2ce47e80-e45f-4ea7-823e-4c5e653f96b7" />

En esta captura se muestra el comando utilizado para **construir la imagen y levantar el contenedor de Dovecot** de forma automatizada:

```bash
docker compose -f docker-compose-dovecot.yml up -d --build

```

### **Análisis del comando:**

* **`-f docker-compose-dovecot.yml`**: Especifica el archivo de configuración de Compose que debe utilizar.
* **`up`**: Se encarga de crear y arrancar los servicios definidos.
* **`-d`** (Detached): Ejecuta el contenedor en segundo plano, liberando la terminal.
* **`--build`**: Fuerza la reconstrucción de la imagen antes de arrancar, asegurando que se incluyan los últimos cambios del `Dockerfile` o los archivos de configuración.

### **Resultados visibles en la salida:**

* **Aviso de `version**`: Aparece un `WARN` sobre el formato del archivo, pero no afecta a la ejecución.
* **`[+] Building`**: Muestra el proceso de construcción de la imagen (utiliza capas en caché para mayor velocidad).
* **`[+] up`**: Indica que el contenedor se ha creado y arrancado correctamente.

Este comando es esencial en nuestro flujo de trabajo para aplicar cualquier modificación en la configuración del servidor.

## Documentación de Álvaro Rodriguez - Parte 2 Dovecot

<img width="931" height="385" alt="image" src="https://github.com/user-attachments/assets/d75a4415-ec29-42a6-ad4b-278682512f9c" />

sudo docker compose -f docker-compose-dovecot.yml logs mostrando únicamente un aviso:

<img width="922" height="82" alt="image" src="https://github.com/user-attachments/assets/690dd1d0-b823-4e44-861f-194632f82fd2" />

Estructura de directorios:

<img width="729" height="432" alt="image" src="https://github.com/user-attachments/assets/77af82b2-d482-4647-b719-1b639bbc4587" />

Contenido de dovecot.conf:

<img width="582" height="944" alt="image" src="https://github.com/user-attachments/assets/9b35b215-498d-48a4-8e58-a681d54b6fec" />

Contenido del archivo users:

<img width="848" height="127" alt="image" src="https://github.com/user-attachments/assets/f2e75637-627a-44b3-bd7f-bdaad6d89345" />

Conexión IMAP exitosa

<img width="931" height="304" alt="image" src="https://github.com/user-attachments/assets/cbdff0ec-d009-47b5-917b-12b943597eaf" />




```markdown
# Práctica: Configuración del Servicio de Correo Electrónico

[cite_start]Este repositorio contiene la documentación y archivos de configuración para el despliegue de un servidor de correo en el dominio `usuario.fpinfo.com.es`[cite: 12].

## 1. Objetivos y Requisitos
* [cite_start]Configuración del servidor SMTP Postfix[cite: 6].
* [cite_start]Configuración del servidor POP/IMAP Dovecot[cite: 6].
* [cite_start]Implementación de seguridad mediante certificados TLS (Autofirmados o Let's Encrypt)[cite: 88, 122].
* [cite_start]Verificación del servicio con `mailutils` y el cliente gráfico Thunderbird[cite: 376, 397].

**Requisitos previos:**
* [cite_start]Registro MX apuntando a `mail.usuario.fpinfo.com.es`[cite: 17].
* [cite_start]Puertos abiertos: 25, 465, 587 (SMTP), 993 (IMAPS), 995 (POP3S)[cite: 18].

---

## 2. Instalación y Configuración de Postfix (SMTP)

### Instalación
```bash
[cite_start]sudo apt update && sudo apt upgrade -y [cite: 21]
[cite_start]sudo apt install postfix -y [cite: 22]

```

### Configuración principal (`/etc/postfix/main.cf`)

Durante el `dpkg-reconfigure postfix`, se establecieron los siguientes parámetros:

* 
**Tipo de configuración:** Internet Site.


* 
**Nombre del sistema:** `usuario.fpinfo.com.es`.


* 
**Format de buzón:** mbox.


* 
**Redes autorizadas (Relay):** `127.0.0.0/8`, `10.0.100.0/24`, `10.100.0.0/16`.



### Seguridad TLS (Certificado Autofirmado)

Generación del certificado para el servidor:

```bash
openssl req -new -newkey rsa:2048 -days 3650 -nodes -x509 \
-subj "/C=ES/ST=Madrid/L=Madrid/O=SR2A/CN=mail.usuario.fpinfo.com.es" \
-keyout /etc/postfix/certs/postfix.pem -out /etc/postfix/certs/postfix.pem

```

---

## 3. Instalación y Configuración de Dovecot (IMAP/POP3)

### Instalación

```bash
[cite_start]sudo apt install dovecot-core dovecot-imapd dovecot-pop3d -y [cite: 301]

```

### Sincronización de buzones (mbox)

En `/etc/dovecot/conf.d/10-mail.conf`:
`mail_location = mbox:~/mail:INBOX=/var/mail/%u` 

### Configuración SSL

Se vincula el certificado generado anteriormente en `/etc/dovecot/conf.d/10-ssl.conf`:

* 
`ssl = yes` 


* 
`ssl_cert = </etc/dovecot/private/dovecot.pem` 


* 
`ssl_key = </etc/dovecot/private/dovecot.pem` 



---

## 4. Verificación y Pruebas

### Comprobación de puertos

```bash
[cite_start]ss -ltnp | grep -E 'master|dovecot' [cite: 81, 373]

```

### Prueba de envío con openssl

Conexión cifrada al puerto 25 con STARTTLS:

```bash
[cite_start]openssl s_client -connect 10.0.100.1:25 -starttls smtp -servername mail.usuario.fpinfo.com.es [cite: 171, 223]

```

### Prueba con Mailutils

Uso del comando `mail` para leer y gestionar el buzón local:

* 
`h`: Listar headers.


* 
`m usuario`: Enviar correo.


* 
`q`: Salir y guardar cambios.



---

## 5. Cliente Thunderbird

Configuración final del cliente:

* 
**Entrada (IMAP):** Puerto 993, SSL/TLS.


* 
**Salida (SMTP):** Puerto 587, STARTTLS.



#### 1. Preparación del entorno
- Máquina virtual o física con Ubuntu (o cualquier Linux con Docker instalado).  
- Instalar Docker y Docker Compose.  
- (Opcional) Instalar `gh` (GitHub CLI) si usas GitHub.

#### 2. Estructura de directorios
Crea una carpeta para el proyecto, por ejemplo `correo-recuperacion`, con subcarpetas:
```
correo-recuperacion/
├── docker-compose.yml
├── postfix/
│   ├── Dockerfile (si optas por imagen propia)
│   └── main.cf (configuración personalizada)
├── dovecot/
│   ├── Dockerfile (si optas por imagen propia)
│   └── dovecot.conf (configuración)
└── maildata/ (volumen para buzones)
```

#### 3. Configuración de Postfix (SMTP)
- Usa la imagen oficial `postfix:latest` o construye una con tu propio `main.cf`.  
- Parámetros esenciales en `main.cf` (basado en la práctica `ASIR_Práctica_Correo_v2.pdf`):
  ```
  myhostname = mail.tudominio.fpinfo.com.es
  mydomain = tudominio.fpinfo.com.es
  myorigin = $mydomain
  mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
  mynetworks = 127.0.0.0/8 10.0.0.0/8 (ajusta a tu red)
  home_mailbox =   (para formato mbox)
  mailbox_size_limit = 20000000
  ```
- Habilita TLS (puedes usar certificado autofirmado para pruebas).  
- Asegura que Postfix escuche en puertos 25 y 587.

#### 4. Configuración de Dovecot (IMAP/POP3)
- Usa imagen oficial `dovecot/dovecot:latest` o crea la tuya.  
- Configura `mail_location = mbox:~/mail:INBOX=/var/mail/%u` (para usar mbox).  
- Habilita los protocolos imap, pop3 (y sus versiones seguras).  
- Integra TLS con el mismo certificado autofirmado.  
- Permite autenticación con contraseña en texto plano para pruebas (ajusta `10-auth.conf`).

#### 5. Docker Compose
Crea `docker-compose.yml` similar a:
```yaml
version: '3.8'
services:
  postfix:
    image: postfix:latest   # o build: ./postfix
    container_name: postfix
    ports:
      - "25:25"
      - "587:587"
    volumes:
      - ./maildata:/var/mail
      - ./postfix/main.cf:/etc/postfix/main.cf
    environment:
      - DOMAIN=tudominio.fpinfo.com.es
    networks:
      - mailnet

  dovecot:
    image: dovecot/dovecot:latest   # o build: ./dovecot
    container_name: dovecot
    ports:
      - "143:143"
      - "993:993"
      - "110:110"
      - "995:995"
    volumes:
      - ./maildata:/var/mail
      - ./dovecot/dovecot.conf:/etc/dovecot/dovecot.conf
    depends_on:
      - postfix
    networks:
      - mailnet

networks:
  mailnet:
    driver: bridge
```

#### 6. Pruebas básicas
- Levanta los contenedores: `docker-compose up -d`  
- Comprueba que escuchan: `docker-compose ps`  
- Envía un correo con `swaks` (instalado en el host) o `telnet`:
  ```
  swaks --to usuario@tudominio.fpinfo.com.es --server localhost
  ```
- Verifica que el correo llega al buzón: `docker exec -it dovecot cat /var/mail/usuario`

#### 7. Prueba con Thunderbird -- Lucas 
- Instala Thunderbird en tu equipo (o en otra máquina de la misma red).  
- Configura una cuenta:
  - **Servidor IMAP**: `mail.tudominio.fpinfo.com.es`, puerto 993, SSL/TLS, contraseña normal.  
  - **Servidor SMTP**: mismo servidor, puerto 587, STARTTLS, contraseña normal.  
- Acepta el certificado autofirmado.  
- Envía un correo entre dos usuarios (puedes crear dos cuentas locales en el servidor, ej. `user1` y `user2`).


1. INTRODUCCIÓN Y OBJETIVOS
La presente documentación corresponde al Paso 7 de la práctica grupal de recuperación del RA5: "Servicio de Correo Electrónico (Ampliación de SRI)". Mi responsabilidad específica dentro del grupo ha sido la realización de las pruebas de funcionamiento con el cliente de correo Thunderbird, verificando que la infraestructura de servidores de correo (Postfix y Dovecot) desplegada mediante contenedores Docker es capaz de proporcionar un servicio de correo electrónico funcional y accesible desde un cliente estándar.

Objetivos específicos de mi parte:

Verificar que el servidor IMAP (Dovecot) permite la conexión de clientes para lectura de correos

Verificar que el servidor SMTP (Postfix) permite el envío de correos con autenticación

Configurar Thunderbird como cliente de correo para dos usuarios del sistema

Realizar pruebas de envío y recepción entre ambos usuarios

Documentar todo el proceso con capturas de pantalla y explicaciones detalladas

Identificar y resolver los problemas técnicos surgidos durante el proceso

2. CONTEXTO DEL PROYECTO Y MI ROL ESPECÍFICO
Este trabajo se enmarca en una práctica grupal donde diferentes miembros del equipo han abordado distintas partes de la implementación de un servidor de correo completo. La arquitectura general del proyecto consiste en:

Servidor SMTP: Postfix en contenedor Docker para envío de correos

Servidor IMAP/POP3: Dovecot en contenedor Docker para acceso a buzones

Persistencia: Volúmenes Docker para almacenamiento de correos

Orquestación: Docker Compose para la gestión de contenedores

Mi rol específico, Paso 7, es el de verificación funcional final, actuando como el "usuario final" que va a utilizar el sistema una vez que los servidores están desplegados. Esta fase es crítica porque demuestra que toda la infraestructura subyacente (configuraciones de Postfix, configuraciones de Dovecot, redes Docker, volúmenes persistentes, autenticación de usuarios) funciona de manera integrada y no solo de forma aislada.


3. VERIFICACIÓN DEL ESTADO INICIAL DEL SERVIDOR
3.1 Comprobación de contenedores Docker
Lo primero que necesitaba saber era qué contenedores relacionados con el servicio de correo estaban ya funcionando en la máquina virtual:

lucas@mail:~$ sudo docker ps
CONTAINER ID    IMAGE    COMMAND    CREATED    STATUS    PORTS
    4b68a6330634   compose-dovecot    "dovecot -F"    17 hours ago    Up 17 hours    0.0.0.0:110->110/tcp, [::]:110->110/tcp, 0.0.0.0:443->143/tcp, [::]:143->143/tcp, 0.0.0.0:993->993/tcp, [::]:993->993/tcp, 0.0.0.0:995->995/tcp, [::]:995->995/tcp    dovecot-grupo6
3543ed87e5a8   jitsi/web:unstable    "/init"    2 weeks ago    Up 2 days    0.0.0.0:443->443/tcp, 0.0.0.0:80->80/tcp, [::]:800->80/tcp
    docker-jitsi-meet-web-1
bc52b62c1eab   jitsi/jvb:unstable    "/init"    2 weeks ago    Up 2 days    127.0.0.1:8080->8080/tcp, 0.0.0.0:10000->10000/udp, [::]:10000->10000/udp
    docker-jitsi-meet-jvb-1
a80c80b26cbc   jitsi/prosody:unstable    "/init"    2 weeks ago    Up 2 days    5222/tcp, 5269/tcp, 5280/tcp, 5347/tcp
    docker-jitsi-meet-prosody-1
7d5649e41e1b   jitsi/jicofo:unstable    "/init"    2 weeks ago    Up 2 days    127.0.0.1:8888->8888/tcp
    docker-jitsi-meet-jicofo-1





4. ANÁLISIS DE LA SITUACIÓN ENCONTRADA
4.1 Búsqueda de contenedores detenidos
Para descartar la opción B, listé todos los contenedores, incluyendo los detenidos:

bash
lucas@mail:~$ sudo docker ps -a | grep -i postfix
lucas@mail:~$ 

4.2 Búsqueda del archivo docker-compose.yml
Para entender cómo se había desplegado el dovecot existente y poder desplegar postfix de manera coherente, necesitaba localizar el archivo docker-compose.yml del proyecto:

bash
lucas@mail:~$ sudo find /home -name "docker-compose.yml" 2>/dev/null
/home/sr2a21/Correo-Grupo6/postix-config/docker-compose.yml
/home/sr2a21/docker-jitsi-meet/docker-compose.yml

¡Éxito! El archivo estaba en /home/sr2a21/Correo-Grupo6/postix-config/docker-compose.yml 


4.3 Tuve problemas con los permisos
Para poder trabajar con el proyecto, decidí copiarlo a mi directorio personal usando sudo:

bash
lucas@mail:~$ sudo cp -r /home/sr2a21/Correo-Grupo6 ~/
lucas@mail:~$ sudo chown -R lucas:lucas ~/Correo-Grupo6
lucas@mail:~$ cd ~/Correo-Grupo6/postix-config
lucas@mail:~/Correo-Grupo6/postix-config$ 

5. LOCALIZACIÓN DE LOS CONTENEDORES EXISTENTES
5.1 Inspección detallada del contenedor dovecot
Una vez con acceso al proyecto, necesitaba entender cómo estaba configurado el dovecot existente para poder desplegar postfix de manera compatible:

bash
lucas@mail:~/Correo-Grupo6/postix-config$ sudo docker inspect dovecot-grupo6
Este comando devuelve una gran cantidad de información en formato JSON. Analicemos las partes más relevantes:

6.2 Información de red
json
"Networks": {
    "compose_default": {
        "IPAMConfig": null,
        "Links": null,
        "Aliases": [
            "dovecot-grupo6",
            "dovecot"
        ],
        "NetworkID": "35e6b767a3de58e78b1bfcd1370e6272b3dc61feba952a1d993edb95263aa2bd",
        "EndpointID": "a3c1e5e80b4350ef389beca0c27f9f71d2e24384758660fb11eb5272476339dc",
        "Gateway": "172.19.0.1",
        "IPAddress": "172.19.0.2",
        "IPPrefixLen": 16
    }
}
Figura 6.1: Información de red del contenedor dovecot

Datos clave obtenidos:

Nombre de la red: compose_default

IP interna del contenedor: 172.19.0.2

Gateway: 172.19.0.1

Esta información es fundamental porque postfix necesitará conectarse a dovecot a través de esta red para entregar los correos.

6.3 Información de volúmenes y montajes
json
"Mounts": [
    {
        "Type": "bind",
        "Source": "/home/alvaro/Correo-Grupo6/dovecot/conf/users",
        "Destination": "/etc/dovecot/users",
        "Mode": "ro",
        "RW": false
    },
    {
        "Type": "bind",
        "Source": "/home/alvaro/Correo-Grupo6/maildata",
        "Destination": "/var/mail",
        "Mode": "rw",
        "RW": true
    },
    {
        "Type": "bind",
        "Source": "/home/alvaro/Correo-Grupo6/dovecot/conf/dovecot.conf",
        "Destination": "/etc/dovecot/dovecot.conf",
        "Mode": "ro",
        "RW": false
    }
]


Datos clave obtenidos:

Ruta de los buzones en el host: /home/alvaro/Correo-Grupo6/maildata

Ruta de los buzones en el contenedor: /var/mail

Archivos de configuración: /home/alvaro/Correo-Grupo6/dovecot/conf/

Para que postfix y dovecot compartan los mismos buzones, postfix deberá montar el mismo volumen /home/alvaro/Correo-Grupo6/maildata en /var/mail.

6.4 Información de puertos expuestos
bash
lucas@mail:~/Correo-Grupo6/postix-config$ sudo docker port dovecot-grupo6
110/tcp -> 0.0.0.0:110
110/tcp -> [::]:110
143/tcp -> 0.0.0.0:143
143/tcp -> [::]:143
993/tcp -> 0.0.0.0:993
993/tcp -> [::]:993
995/tcp -> 0.0.0.0:995
995/tcp -> [::]:995

Datos clave:

IMAP estándar (143) y seguro (993) accesibles

POP3 estándar (110) y seguro (995) accesibles

Todos los puertos están mapeados a la interfaz 0.0.0.0, lo que significa que son accesibles desde cualquier interfaz de red

6.5 Información de etiquetas Docker Compose
json
"Labels": {
    "com.docker.compose.config-hash": "f578d2ed2a7d00d65d83bf94828cfca920470e6c80e63258d21c790b7410cf00",
    "com.docker.compose.container-number": "1",
    "com.docker.compose.project": "compose",
    "com.docker.compose.project.config_files": "/home/alvaro/Correo-Grupo6/compose/docker-compose-dovecot.yml",
    "com.docker.compose.project.working_dir": "/home/alvaro/Correo-Grupo6/compose",
    "com.docker.compose.service": "dovecot"
}


Estas etiquetas confirman que el contenedor fue desplegado con Docker Compose desde el directorio /home/alvaro/Correo-Grupo6/compose, usando un archivo específico para dovecot.

6.6 Conclusión del análisis
Tras este exhaustivo análisis, concluí que:

Dovecot está correctamente configurado y funcionando con todos los puertos necesarios

Postfix no está desplegado (no aparece en docker ps ni en docker ps -a)

El volumen de datos está en /home/alvaro/Correo-Grupo6/maildata

La red a utilizar es compose_default

La IP interna de dovecot es 172.19.0.2

Mi tarea ahora es desplegar postfix de manera que se integre con esta infraestructura existente.

7. CREACIÓN DE USUARIOS DE PRUEBA
Para realizar las pruebas con Thunderbird, necesitaba al menos dos usuarios en el sistema que pudieran enviar y recibir correos entre sí.

7.1 Verificación de usuarios existentes
bash
lucas@mail:~/Correo-Grupo6/postix-config$ cat /etc/passwd | grep /home | tail -5
alvaro:x:1001:1001:Alvaro,,,:/home/alvaro:/bin/bash
sr2a21:x:1002:1002:,,,:/home/sr2a21:/bin/bash
lucas:x:1003:1003:,,,:/home/lucas:/bin/bash


No había usuarios de prueba específicos para el correo, así que procedí a crearlos.

7.2 Creación de usuario1
bash
lucas@mail:~/Correo-Grupo6/postix-config$ sudo adduser usuario1
Añadiendo usuario `usuario1' ...
Añadiendo nuevo grupo `usuario1' (1004) ...
Añadiendo nuevo usuario `usuario1' (1004) con grupo `usuario1' ...
Creando el directorio personal `/home/usuario1' ...
Copiando los ficheros desde `/etc/skel' ...
Nueva contraseña: 
Vuelva a escribir la nueva contraseña: 
passwd: contraseña actualizada correctamente
Cambiando la información de usuario para usuario1
Introduzca el nuevo valor, o pulse INTRO para el valor predeterminado
    Nombre completo []: Usuario de Prueba 1
    Número de habitación []: 
    Teléfono del trabajo []: 
    Teléfono de casa []: 
    Otro []: 
¿Es correcta la información? [S/n] S


Detalles del usuario1:

Nombre de usuario: usuario1

UID/GID: 1004/1004

Directorio home: /home/usuario1

Contraseña: (establecida durante el proceso, anotada para las pruebas)

Nombre completo: Usuario de Prueba 1

7.3 Creación de usuario2
bash
lucas@mail:~/Correo-Grupo6/postix-config$ sudo adduser usuario2
Añadiendo usuario `usuario2' ...
Añadiendo nuevo grupo `usuario2' (1005) ...
Añadiendo nuevo usuario `usuario2' (1005) con grupo `usuario2' ...
Creando el directorio personal `/home/usuario2' ...
Copiando los ficheros desde `/etc/skel' ...
Nueva contraseña: 
Vuelva a escribir la nueva contraseña: 
passwd: contraseña actualizada correctamente
Cambiando la información de usuario para usuario2
Introduzca el nuevo valor, o pulse INTRO para el valor predeterminado
    Nombre completo []: Usuario de Prueba 2
    Número de habitación []: 
    Teléfono del trabajo []: 
    Teléfono de casa []: 
    Otro []: 
¿Es correcta la información? [S/n] S


Detalles del usuario2:

Nombre de usuario: usuario2

UID/GID: 1005/1005

Directorio home: /home/usuario2

Contraseña: (establecida durante el proceso, anotada para las pruebas)

Nombre completo: Usuario de Prueba 2

7.4 Verificación de la creación
bash
lucas@mail:~/Correo-Grupo6/postix-config$ cat /etc/passwd | grep usuario
usuario1:x:1004:1004:Usuario de Prueba 1,,,:/home/usuario1:/bin/bash
usuario2:x:1005:1005:Usuario de Prueba 2,,,:/home/usuario2:/bin/bash


8. VERIFICACIÓN DE CONECTIVIDAD Y PUERTOS
Antes de pasar a la configuración de Thunderbird, necesitaba asegurarme de que los servicios eran accesibles desde el exterior.

8.1 Verificación de puertos en escucha
bash
lucas@mail:~/Correo-Grupo6/postix-config$ sudo netstat -tlnp | grep -E ':25|:587|:143|:993|:110|:995'
tcp        0      0 0.0.0.0:110             0.0.0.0:*               LISTEN      224278/dovecot      
tcp        0      0 0.0.0.0:143              0.0.0.0:*               LISTEN      224278/dovecot      
tcp        0      0 0.0.0.0:993              0.0.0.0:*               LISTEN      224278/dovecot      
tcp        0      0 0.0.0.0:995              0.0.0.0:*               LISTEN      224278/dovecot      
tcp        0      0 0.0.0.0:587              0.0.0.0:*               LISTEN      227632/master       
tcp        0      0 0.0.0.0:25               0.0.0.0:*               LISTEN      227632/master       
tcp6       0      0 :::110                   :::*                    LISTEN      224278/dovecot      
tcp6       0      0 :::143                   :::*                    LISTEN      224278/dovecot      
tcp6       0      0 :::993                   :::*                    LISTEN      224278/dovecot      
tcp6       0      0 :::995                   :::*                    LISTEN      224278/dovecot      
tcp6       0      0 :::587                   :::*                    LISTEN      227632/master       
tcp6       0      0 :::25                    :::*                    LISTEN      227632/master


Análisis de la salida:

0.0.0.0:puerto: el servicio escucha en todas las interfaces IPv4

:::puerto: el servicio escucha en todas las interfaces IPv6

LISTEN: el servicio está activo y aceptando conexiones

Los puertos 110, 143, 993, 995 están gestionados por dovecot (PID 224278)

Los puertos 25 y 587 están gestionados por postfix (PID 227632)

8.2 Verificación del firewall
bash
lucas@mail:~/Correo-Grupo6/postix-config$ sudo ufw status
Status: inactive


8.3 Obtención de la IP del servidor
bash
lucas@mail:~/Correo-Grupo6/postix-config$ hostname -I
172.19.0.1 10.0.2.15 192.168.1.45


La IP 192.168.1.45 es la que está en la red local y será accesible desde mi PC para las pruebas con Thunderbird.

8.4 Prueba de conectividad desde la propia máquina
Para confirmar que los servicios responden correctamente:

bash
lucas@mail:~/Correo-Grupo6/postix-config$ telnet localhost 143
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
* OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ STARTTLS AUTH=PLAIN] Dovecot ready.
^]
telnet> quit
Connection closed.


bash
lucas@mail:~/Correo-Grupo6/postix-config$ telnet localhost 587
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
220 mail.feri.fpinto.com.es ESMTP Postfix
^]
telnet> quit
Connection closed.


8.5 Conclusión de las verificaciones
Todos los servicios están funcionando correctamente y son accesibles. Ya puedo proceder con la configuración de Thunderbird.

9. INSTALACIÓN Y CONFIGURACIÓN DE THUNDERBIRD
9.1 Instalación de Thunderbird en el equipo cliente
Desde mi PC local con Windows 11, descargué e instalé Thunderbird desde la página oficial: https://www.thunderbird.net/

Versión instalada: 115.6.0 (64 bits)

9.2 Configuración de la primera cuenta (usuario1)
Al abrir Thunderbird por primera vez, aparece el asistente de configuración de cuentas:

Pantalla inicial: Seleccioné "Añadir cuenta de correo"

Datos básicos:

Nombre: Usuario Prueba 1

Correo: usuario1@feri.fpinto.com.es

Contraseña: [la establecida durante la creación del usuario]

Configuración manual: Al hacer clic en "Continuar", Thunderbird intentó autodetectar la configuración pero falló (lo esperado, ya que es un servidor privado). Hice clic en "Configurar manualmente" en la parte inferior derecha.


https://capturas/thunderbird-config-manual.png

Parámetros introducidos:

Servidor entrante (IMAP)	
Protocolo	IMAP
Nombre del servidor	192.168.1.45
Puerto	143
Conexión	STARTTLS
Autenticación	Contraseña normal
Nombre de usuario	usuario1
Servidor saliente (SMTP)	
Nombre del servidor	192.168.1.45
Puerto	587
Conexión	STARTTLS
Autenticación	Contraseña normal
Nombre de usuario	usuario1
Explicación de cada parámetro:

Protocolo IMAP: Elegí IMAP en lugar de POP3 porque permite gestionar los correos directamente en el servidor, manteniendo la sincronización entre múltiples dispositivos. Esto es más adecuado para un entorno de pruebas donde queremos ver los cambios en tiempo real.

Puerto 143: Es el puerto estándar para IMAP sin cifrar. Aunque usamos STARTTLS, el puerto de conexión inicial es el 143.

STARTTLS: Este método permite iniciar la conexión sin cifrar y luego negociar el cifrado mediante TLS. Es compatible con certificados autofirmados como el que tenemos en el servidor.

Contraseña normal: El método de autenticación básico donde el cliente envía el usuario y contraseña (protegidos por el túnel TLS).

Puerto 587: Es el puerto estándar para envío de correo con autenticación (SMTP submission), a diferencia del puerto 25 que tradicionalmente se usa para transferencia entre servidores.

Aceptación del certificado:

<img width="607" height="729" alt="image" src="https://github.com/user-attachments/assets/ebc263b2-e288-4430-9583-60f962e21c7f" />


# 📧 Paso 7 — Pruebas de Funcionamiento con Thunderbird  
## Servicio de Correo Electrónico — Ampliación de SRI (RA5)

---

## 👤 Autor
**Lucas**

## 📅 Fecha
22 de febrero de 2026  

## 🖥️ Entorno
Máquina virtual **Ubuntu 22.04 LTS**

---

# 📑 Índice

- [1. Introducción y Objetivos](#1-introducción-y-objetivos)
- [2. Contexto del Proyecto y Mi Rol Específico](#2-contexto-del-proyecto-y-mi-rol-específico)
- [3. Verificación del Estado Inicial del Servidor](#3-verificación-del-estado-inicial-del-servidor)
- [4. Análisis de la Situación Encontrada](#4-análisis-de-la-situación-encontrada)
- [5. Localización de los Contenedores Existentes](#5-localización-de-los-contenedores-existentes)
- [6. Análisis Técnico del Contenedor Dovecot](#6-análisis-técnico-del-contenedor-dovecot)
- [7. Creación de Usuarios de Prueba](#7-creación-de-usuarios-de-prueba)
- [8. Verificación de Conectividad y Puertos](#8-verificación-de-conectividad-y-puertos)
- [9. Instalación y Configuración de Thunderbird](#9-instalación-y-configuración-de-thunderbird)

---

# 1️⃣ Introducción y Objetivos

La presente documentación corresponde al **Paso 7** de la práctica grupal de recuperación del **RA5: Servicio de Correo Electrónico (Ampliación de SRI)**.

Mi responsabilidad dentro del grupo ha sido realizar las pruebas de funcionamiento utilizando el cliente de correo **Thunderbird**, verificando que la infraestructura compuesta por **Postfix + Dovecot en Docker** proporciona un servicio funcional y accesible.

---

## 🎯 Objetivos específicos

- ✅ Verificar que el servidor **IMAP (Dovecot)** permite la lectura de correos  
- ✅ Verificar que el servidor **SMTP (Postfix)** permite el envío autenticado  
- ✅ Configurar Thunderbird para dos usuarios  
- ✅ Realizar pruebas de envío y recepción  
- ✅ Documentar todo el proceso  
- ✅ Detectar y resolver incidencias técnicas  

---

# 2️⃣ Contexto del Proyecto y Mi Rol Específico

## 🏗️ Arquitectura General

| Componente | Tecnología | Función |
|------------|------------|----------|
| Servidor SMTP | Postfix (Docker) | Envío de correos |
| Servidor IMAP/POP3 | Dovecot (Docker) | Acceso a buzones |
| Persistencia | Volúmenes Docker | Almacenamiento |
| Orquestación | Docker Compose | Gestión de contenedores |

---

## 👤 Mi Rol — Verificación Funcional Final

Actúo como **usuario final del sistema**, comprobando que toda la infraestructura funciona de forma integrada:

- Configuración de Postfix  
- Configuración de Dovecot  
- Redes Docker  
- Volúmenes persistentes  
- Autenticación de usuarios  

> 🔎 Esta fase demuestra que el sistema no solo funciona técnicamente, sino que es utilizable en un entorno real.

---

# 3️⃣ Verificación del Estado Inicial del Servidor

## 3.1 Comprobación de contenedores Docker

```bash
lucas@mail:~$ sudo docker ps
```

```text
CONTAINER ID   IMAGE              COMMAND         STATUS       PORTS
4b68a6330634   compose-dovecot    "dovecot -F"    Up 17 hours  110,143,993,995
...
```

📌 Observaciones:

- Dovecot está activo
- No aparece ningún contenedor de Postfix
- Existen contenedores de Jitsi (no relevantes)

---

# 4️⃣ Análisis de la Situación Encontrada

## 4.1 Búsqueda de contenedores detenidos

```bash
sudo docker ps -a | grep -i postfix
```

Resultado:  
No se encontró ningún contenedor de Postfix.

---

## 4.2 Localización del docker-compose.yml

```bash
sudo find /home -name "docker-compose.yml" 2>/dev/null
```

```text
/home/sr2a21/Correo-Grupo6/postix-config/docker-compose.yml
```

✔ Proyecto localizado correctamente.

---

## 4.3 Problemas de permisos

```bash
sudo cp -r /home/sr2a21/Correo-Grupo6 ~/
sudo chown -R lucas:lucas ~/Correo-Grupo6
cd ~/Correo-Grupo6/postix-config
```

✔ Proyecto listo para modificación.

---

# 5️⃣ Localización de los Contenedores Existentes

## 5.1 Inspección de Dovecot

```bash
sudo docker inspect dovecot-grupo6
```

---

# 6️⃣ Análisis Técnico del Contenedor Dovecot

## 6.2 Información de Red

```json
"Networks": {
  "compose_default": {
    "Gateway": "172.19.0.1",
    "IPAddress": "172.19.0.2"
  }
}
```

**Datos clave:**

- Red: `compose_default`
- IP interna: `172.19.0.2`
- Gateway: `172.19.0.1`

---

## 6.3 Volúmenes Compartidos

```json
"Source": "/home/alvaro/Correo-Grupo6/maildata",
"Destination": "/var/mail"
```

📌 Postfix deberá montar el mismo volumen para compartir buzones.

---

## 6.4 Puertos Expuestos

```bash
sudo docker port dovecot-grupo6
```

| Puerto | Servicio |
|--------|----------|
| 110 | POP3 |
| 143 | IMAP |
| 993 | IMAPS |
| 995 | POP3S |

Todos accesibles desde `0.0.0.0`.

---

# 7️⃣ Creación de Usuarios de Prueba

## 7.1 Verificación previa

```bash
cat /etc/passwd | grep /home | tail -5
```

---

## 7.2 Creación de usuario1

```bash
sudo adduser usuario1
```

| Campo | Valor |
|-------|-------|
| Usuario | usuario1 |
| UID/GID | 1004 |
| Home | /home/usuario1 |

---

## 7.3 Creación de usuario2

```bash
sudo adduser usuario2
```

| Campo | Valor |
|-------|-------|
| Usuario | usuario2 |
| UID/GID | 1005 |
| Home | /home/usuario2 |

---

# 8️⃣ Verificación de Conectividad y Puertos

## 8.1 Servicios en escucha

```bash
sudo netstat -tlnp | grep -E ':25|:587|:143|:993|:110|:995'
```

### Resultado

| Puerto | Servicio |
|--------|----------|
| 110,143,993,995 | Dovecot |
| 25,587 | Postfix |

---

## 8.2 Estado del Firewall

```bash
sudo ufw status
```

```text
Status: inactive
```

---

## 8.3 IP del Servidor

```bash
hostname -I
```

```text
192.168.1.45
```

✔ IP utilizada para pruebas con Thunderbird.

---

## 8.4 Prueba con Telnet

```bash
telnet localhost 143
```

```text
* OK Dovecot ready.
```

```bash
telnet localhost 587
```

```text
220 mail.feri.fpinto.com.es ESMTP Postfix
```

✔ Servicios operativos correctamente.

---

# 9️⃣ Instalación y Configuración de Thunderbird

## 9.1 Instalación

Descargado desde:  
https://www.thunderbird.net/

Versión instalada: **115.6.0 (64 bits)**

---

## 9.2 Configuración de la Cuenta `usuario1`

### 📥 Servidor Entrante (IMAP)

| Parámetro | Valor |
|------------|--------|
| Servidor | 192.168.1.45 |
| Puerto | 143 |
| Seguridad | STARTTLS |
| Autenticación | Contraseña normal |
| Usuario | usuario1 |

---

### 📤 Servidor Saliente (SMTP)

| Parámetro | Valor |
|------------|--------|
| Servidor | 192.168.1.45 |
| Puerto | 587 |
| Seguridad | STARTTLS |
| Autenticación | Contraseña normal |
| Usuario | usuario1 |

---

## 🔐 Explicación Técnica

- **IMAP (143 + STARTTLS):** Permite sincronización en servidor.
- **SMTP (587):** Puerto estándar de envío autenticado.
- **STARTTLS:** Negocia cifrado TLS tras conexión inicial.
- **Contraseña normal:** Credenciales protegidas por TLS.

---

# ✅ Conclusión

El sistema de correo:

- ✔ Permite autenticación correcta  
- ✔ Permite envío y recepción de correos  
- ✔ Tiene puertos correctamente expuestos  
- ✔ Funciona desde cliente externo (Thunderbird)  

La infraestructura Docker (Postfix + Dovecot + Volúmenes + Red) funciona de forma integrada y estable.

---

# 🏁 Estado Final

**Infraestructura validada y operativa.**



