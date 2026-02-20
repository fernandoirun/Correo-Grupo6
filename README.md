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

---

### 📌 Recursos de los archivos proporcionados

- **Guía práctica detallada**: `ASIR_Práctica_Correo_v2.pdf` (paso a paso para instalar Postfix y Dovecot en una máquina real, con comandos y configuraciones).  
- **Guía ABP con Docker**: `ASIR_ABP_CorreoE(RA5)_v1.pdf` (orientación sobre contenedores, Dockerfile, Compose, GitHub y Terraform).  
- **Transparencias de teoría**: `ASIR_Transparencias_Correo_v2 (1).pdf` (fundamentos de correo, protocolos, agentes, seguridad).  

Utiliza estos documentos como base para completar tu práctica.

---

### ⏰ Fecha de entrega

La entrega se realizará en las fechas establecidas para la recuperación extraordinaria. Asegúrate de subir el **PDF** (con todas las pruebas) al lugar indicado por tu profesora.

Si tienes dudas concretas sobre algún paso, no dudes en preguntar. ¡Mucho ánimo!
