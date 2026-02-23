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








# ASIR-Practica-Correo-FDLA
Repositorio de Fernando Irún, Daniel Estévez, Lucas Garcia Redruello, Álvaro Rodríguez para la implementación y configuración de un sistema de correo electrónico completo (MTA y MDA) sobre Ubuntu Server, utilizando Postfix para el envío mediante SMTP y Dovecot para el acceso a buzones mediante IMAP/POP3, integrando seguridad TLS y pruebas de cliente con Thunderbird


Para tu repositorio de GitHub, la descripción debe ser profesional y directa, indicando que se trata de un entorno de laboratorio para el despliegue de servicios de red.

### Descripción del repositorio

> **Repositorio para la implementación y configuración de un sistema de correo electrónico completo (MTA y MDA) sobre Ubuntu Server, utilizando Postfix para el envío mediante SMTP y Dovecot para el acceso a buzones mediante IMAP/POP3, integrando seguridad TLS y pruebas de cliente con Thunderbird.**

---

### Borrador de `README.md`

A continuación tienes una estructura profesional basada específicamente en los pasos y comandos de tu PDF.

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



```

¿Te gustaría que te ayude a redactar un pequeño script en Bash para automatizar la parte de la instalación de los paquetes?

```


## 📧 Recuperación del RA5: Servicio de Correo Electrónico (Ampliación de SRI)

Según la información proporcionada, la **recuperación extraordinaria** del RA5 (y su ampliación) consiste en realizar de **forma individual** la práctica del servicio de correo electrónico, empleando algunas de las tecnologías vistas en el módulo de Ampliación de SRI (Servicios de Red e Internet). El resultado debe documentarse en un **PDF** que incluya las pruebas realizadas.

A continuación, te resumo los **objetivos clave**, los **pasos a seguir** y los **criterios de evaluación** para que puedas preparar tu trabajo de recuperación con éxito.

---

### 🎯 Objetivos del proyecto (según el ABP)

1. **Trabajar con Git/GitHub** (opcional, pero recomendado para la ampliación)  
   - Crear un repositorio privado (invita a la profesora `mercerodri` como colaboradora).  
   - Usar ramas, commits y, si procede, pull requests.  

2. **Implementar un servidor de correo con contenedores Docker**  
   - **Postfix** (SMTP) y **Dovecot** (IMAP/POP3) en contenedores independientes.  
   - Puedes usar imágenes oficiales (`postfix:latest`, `dovecot/dovecot:latest`) o crear tus propias imágenes con `Dockerfile`.  
   - Los buzones deben ser persistentes (volúmenes).  

3. **Automatizar el despliegue con Docker Compose**  
   - Definir los servicios, puertos (25, 587, 143, 993, 110, 995), volúmenes y dependencias.  
   - Asegurar que los contenedores se comuniquen correctamente.  

4. **Investigar / opcionalmente implementar despliegue en Azure con Terraform**  
   - Se valora positivamente si exploras cómo llevar los contenedores a Azure (ACI o VM).  

5. **Realizar pruebas funcionales**  
   - Envío y recepción de correos entre dos usuarios locales.  
   - Usar cliente de correo (Thunderbird) configurado con **IMAP** y **SMTP autenticado**.  
   - Verificar logs, buzones (`/var/mail`), y funcionamiento con comandos como `mail` o `swaks`.  

6. **Documentar todo en un PDF**  
   - Incluir capturas de pantalla de:  
     - Configuración de contenedores y `docker-compose.yml`.  
     - Pruebas de envío/recepción (Thunderbird, `mail`, logs).  
     - (Opcional) Pasos en GitHub y Terraform.  
   - Explicar brevemente cada etapa y los problemas encontrados.  

---

### 📋 Pasos recomendados para la recuperación

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

#### 7. Prueba con Thunderbird
- Instala Thunderbird en tu equipo (o en otra máquina de la misma red).  
- Configura una cuenta:
  - **Servidor IMAP**: `mail.tudominio.fpinfo.com.es`, puerto 993, SSL/TLS, contraseña normal.  
  - **Servidor SMTP**: mismo servidor, puerto 587, STARTTLS, contraseña normal.  
- Acepta el certificado autofirmado.  
- Envía un correo entre dos usuarios (puedes crear dos cuentas locales en el servidor, ej. `user1` y `user2`).

El objetivo de esta parte de la práctica es verificar que el servidor de correo (Postfix + Dovecot) es funcional desde la perspectiva de un usuario final, utilizando un cliente de correo gráfico como Thunderbird. Se comprobará el envío y recepción de correos electrónicos entre dos usuarios del sistema.

Comprobar contenedores en ejecución
root@mail:~# docker ps | grep -E "postfix|dovecot"

Se crearon dos usuarios locales en el sistema para realizar las pruebas de envío y recepción:
root@mail:~# sudo adduser usuario1
root@mail:~# sudo adduser usuario2

root@mail:~# hostname -I
172.19.0.1 10.0.2.15 192.168.1.45

Desde un equipo cliente (Windows 11) se procedió a configurar Thunderbird:

<img width="607" height="729" alt="image" src="https://github.com/user-attachments/assets/ebc263b2-e288-4430-9583-60f962e21c7f" />



#### 8. Documentación en PDF
- Incluye:
  - Objetivo de la práctica.  
  - Explicación de la arquitectura (Postfix, Dovecot, Docker).  
  - Pasos realizados (comandos, configuraciones).  
  - Capturas de pantalla de:
    - Archivos de configuración relevantes.  
    - `docker-compose.yml`.  
    - Ejecución de `docker-compose up` y `docker ps`.  
    - Envío y recepción de correo (Thunderbird o `mail`).  
    - Logs que demuestren el funcionamiento.  
  - Problemas encontrados y soluciones.  
  - Conclusiones.

---

### ✅ Criterios de evaluación (basados en la rúbrica del ABP)

La evaluación se centra en:

- **Funcionalidad del servicio**: Postfix y Dovecot correctamente instalados y configurados, envío/recepción funcionando.  
- **Uso de contenedores**: imágenes adecuadas, persistencia de datos, red entre contenedores.  
- **Automatización con Docker Compose**: correcto uso de dependencias, puertos, volúmenes.  
- **Pruebas completas**: demo con Thunderbird, logs, etc.  
- **Calidad de la documentación**: claridad, capturas, explicaciones técnicas.  
- **Ampliación**: uso de GitHub (commits, ramas) y Terraform (si se implementa).
