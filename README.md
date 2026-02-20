# ASIR-Practica-Correo-FDLA
Repositorio de Fernando Irún, Daniel Estévez, Lucas, Álvaro Rodríguez para la implementación y configuración de un sistema de correo electrónico completo (MTA y MDA) sobre Ubuntu Server, utilizando Postfix para el envío mediante SMTP y Dovecot para el acceso a buzones mediante IMAP/POP3, integrando seguridad TLS y pruebas de cliente con Thunderbird


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
