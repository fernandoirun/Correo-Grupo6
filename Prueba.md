## 📋 **ÍNDICE DE TAREAS**

1. [Preparar la máquina Azure](#1-preparar-la-máquina-azure-grupo-6)
2. [Configurar DNS y dominios](#2-configurar-dns-y-dominios-grupo-6)
3. [Clonar el repositorio GitHub](#3-clonar-el-repositorio-github)
4. [Estructura del proyecto](#4-estructura-del-proyecto)
5. [Configurar Postfix con Docker](#5-configurar-postfix-con-docker)
6. [Configurar Dovecot con Docker](#6-configurar-dovecot-con-docker)
7. [Crear docker-compose.yml](#7-crear-docker-composeyml)
8. [Crear usuarios de prueba](#8-crear-usuarios-de-prueba)
9. [Levantar los servicios](#9-levantar-los-servicios)
10. [Pruebas iniciales](#10-pruebas-iniciales)
11. [Configurar Thunderbird](#11-configurar-thunderbird)
12. [Git: Commit y push](#12-git-commit-y-push)
13. [Documentación para el PDF](#13-documentación-para-el-pdf)

---

## 1. **Preparar la máquina Azure - Grupo 6**

### 1.1 Conectarse a la máquina
```bash
# Conéctate por SSH a tu máquina Azure
ssh tu_usuario@IP_PUBLICA_AZURE
```

### 1.2 Actualizar el sistema
```bash
sudo apt update && sudo apt upgrade -y
```

### 1.3 Instalar Docker y Docker Compose
```bash
# Instalar Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Añadir tu usuario al grupo docker
sudo usermod -aG docker $USER

# Cerrar sesión y volver a entrar (o ejecutar:)
newgrp docker

# Instalar Docker Compose
sudo apt install -y docker-compose-plugin

# Verificar instalaciones
docker --version
docker compose version
```

### 1.4 Instalar herramientas útiles
```bash
sudo apt install -y git tree nano swaks mailutils
```

---

## 2. **Configurar DNS y dominios - Grupo 6**

### 2.1 Definir variables
```bash
export DOMINIO="grupo6.fpinfo.com.es"
export HOSTNAME="mail.${DOMINIO}"
export IP_PUBLICA="TU_IP_PUBLICA_DE_AZURE"

# Verificar
echo "Dominio: $DOMINIO"
echo "Hostname: $HOSTNAME"
echo "IP: $IP_PUBLICA"
```

### 2.2 Configurar hostname
```bash
sudo hostnamectl set-hostname $HOSTNAME

# Editar /etc/hosts
echo "127.0.1.1 $HOSTNAME $DOMINIO" | sudo tee -a /etc/hosts
echo "$IP_PUBLICA $HOSTNAME" | sudo tee -a /etc/hosts
```

### 2.3 **Registros DNS necesarios**
Ve a tu proveedor DNS y configura:

| Registro | Tipo | Valor |
|----------|------|-------|
| grupo6.fpinfo.com.es | A | TU_IP_PUBLICA_AZURE |
| mail.grupo6.fpinfo.com.es | A | TU_IP_PUBLICA_AZURE |
| grupo6.fpinfo.com.es | MX | mail.grupo6.fpinfo.com.es (prioridad 10) |

> ⚠️ **Espera 5-10 minutos** a que se propaguen los DNS

### 2.4 Verificar DNS
```bash
# Desde tu máquina Azure
nslookup $DOMINIO
nslookup $HOSTNAME
dig $DOMINIO MX
```

---

## 3. **Clonar el repositorio GitHub**

```bash
# Clonar el repositorio del grupo 6
git clone https://github.com/tu-organizacion/correo-grupo6.git
cd correo-grupo6

# Ver el contenido
ls -la
```

---

## 4. **Estructura del proyecto**

### 4.1 Crear directorios necesarios
```bash
# Crear estructura de directorios
mkdir -p postfix/certs dovecot/private maildata compose terraform

# Ver la estructura
tree
```

Deberías ver algo como:
```
correo-grupo6/
├── postfix/
│   └── certs/
├── dovecot/
│   └── private/
├── maildata/
├── compose/
└── terraform/
```

---

## 5. **Configurar Postfix con Docker**

### 5.1 Crear configuración de Postfix
```bash
cd ~/correo-grupo6/postfix
nano main.cf
```

```bash
# IDENTIDAD DEL SERVIDOR
myhostname = mail.grupo6.fpinfo.com.es
mydomain = grupo6.fpinfo.com.es
myorigin = $mydomain

# DOMINIOS LOCALES
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain

# REDES AUTORIZADAS (importante para Azure)
mynetworks = 127.0.0.0/8 10.0.0.0/8 172.16.0.0/12 168.63.129.16/32

# BUZONES (formato mbox)
home_mailbox =
mail_spool_directory = /var/mail
mailbox_size_limit = 20000000

# INTERFACES
inet_interfaces = all
inet_protocols = ipv4

# TLS CONFIGURATION
smtpd_tls_cert_file = /etc/postfix/certs/postfix.pem
smtpd_tls_key_file = /etc/postfix/certs/postfix.pem
smtpd_use_tls = yes
smtpd_tls_auth_only = no
smtpd_tls_security_level = may
smtpd_tls_loglevel = 1
smtpd_tls_received_header = yes

# OUTBOUND TLS
smtp_tls_security_level = may
smtp_tls_cafile = /etc/ssl/certs/ca-certificates.crt
```

### 5.2 Crear certificado SSL autofirmado
```bash
cd ~/correo-grupo6/postfix/certs

openssl req -new -newkey rsa:2048 -days 3650 -nodes \
  -x509 -subj "/C=ES/ST=Madrid/L=Madrid/O=Grupo6/CN=mail.grupo6.fpinfo.com.es" \
  -keyout postfix.pem -out postfix.pem

chmod 600 postfix.pem

# Verificar que se creó
ls -la postfix.pem
```

### 5.3 Crear Dockerfile para Postfix (opcional)
```bash
cd ~/correo-grupo6/postfix
nano Dockerfile
```

```dockerfile
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    postfix \
    mailutils \
    libsasl2-modules \
    && rm -rf /var/lib/apt/lists/*

# Crear directorios necesarios
RUN mkdir -p /etc/postfix/certs /var/mail

# Copiar configuración
COPY main.cf /etc/postfix/main.cf
COPY certs/postfix.pem /etc/postfix/certs/postfix.pem

# Permisos
RUN chmod 600 /etc/postfix/certs/postfix.pem && \
    chmod 775 /var/mail

VOLUME /var/mail

EXPOSE 25 587

CMD ["postfix", "start-fg"]
```

---

## 6. **Configurar Dovecot con Docker**

### 6.1 Crear configuración de Dovecot
```bash
cd ~/correo-grupo6/dovecot
nano dovecot.conf
```

```bash
# Protocolos
protocols = imap pop3

# Localización de buzones (mbox)
mail_location = mbox:~/mail:INBOX=/var/mail/%u

# Autenticación
auth_mechanisms = plain login
disable_plaintext_auth = no

# SSL/TLS
ssl = yes
ssl_cert = </etc/dovecot/private/dovecot.pem
ssl_key = </etc/dovecot/private/dovecot.pem

# Servicios
service imap-login {
  inet_listener imap {
    port = 143
  }
  inet_listener imaps {
    port = 993
    ssl = yes
  }
}

service pop3-login {
  inet_listener pop3 {
    port = 110
  }
  inet_listener pop3s {
    port = 995
    ssl = yes
  }
}

# Usuarios del sistema
userdb {
  driver = passwd
}

passdb {
  driver = pam
}

# Logging
log_path = /var/log/dovecot.log
info_log_path = /var/log/dovecot-info.log
debug_log_path = /var/log/dovecot-debug.log
```

### 6.2 Copiar certificado para Dovecot
```bash
cd ~/correo-grupo6/dovecot/private
cp ../../postfix/certs/postfix.pem dovecot.pem
chmod 600 dovecot.pem
```

### 6.3 Crear Dockerfile para Dovecot
```bash
cd ~/correo-grupo6/dovecot
nano Dockerfile
```

```dockerfile
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    dovecot-core \
    dovecot-imapd \
    dovecot-pop3d \
    && rm -rf /var/lib/apt/lists/*

# Crear directorios
RUN mkdir -p /etc/dovecot/private /var/mail /var/log

# Copiar configuración
COPY dovecot.conf /etc/dovecot/dovecot.conf
COPY private/dovecot.pem /etc/dovecot/private/dovecot.pem

# Permisos
RUN chmod 600 /etc/dovecot/private/dovecot.pem && \
    chmod 775 /var/mail

VOLUME /var/mail

EXPOSE 143 993 110 995

CMD ["dovecot", "-F"]
```

---

## 7. **Crear docker-compose.yml**

```bash
cd ~/correo-grupo6/compose
nano docker-compose.yml
```

```yaml
version: '3.8'

services:
  postfix:
    build: ../postfix
    container_name: postfix-grupo6
    hostname: mail.grupo6.fpinfo.com.es
    ports:
      - "25:25"
      - "587:587"
    volumes:
      - ../maildata:/var/mail
      - ../postfix/main.cf:/etc/postfix/main.cf
      - ../postfix/certs:/etc/postfix/certs
    networks:
      - mail-network-grupo6
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  dovecot:
    build: ../dovecot
    container_name: dovecot-grupo6
    ports:
      - "143:143"
      - "993:993"
      - "110:110"
      - "995:995"
    volumes:
      - ../maildata:/var/mail
      - ../dovecot/dovecot.conf:/etc/dovecot/dovecot.conf
      - ../dovecot/private:/etc/dovecot/private
    depends_on:
      - postfix
    networks:
      - mail-network-grupo6
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

networks:
  mail-network-grupo6:
    driver: bridge
```

### 7.1 Crear archivo .env (opcional)
```bash
nano .env
```

```bash
DOMINIO=grupo6.fpinfo.com.es
HOSTNAME=mail.grupo6.fpinfo.com.es
```

---

## 8. **Crear usuarios de prueba**

```bash
# Crear dos usuarios para pruebas
sudo useradd -m usuario1
sudo useradd -m usuario2

# Establecer contraseñas (apunta las contraseñas)
sudo passwd usuario1
# (pon una contraseña, ej: Usuario1_2024)

sudo passwd usuario2
# (pon una contraseña, ej: Usuario2_2024)

# Añadir a grupo mail
sudo usermod -aG mail usuario1
sudo usermod -aG mail usuario2
sudo usermod -aG mail $USER

# Verificar usuarios
id usuario1
id usuario2

# Crear directorio de buzón si no existe
sudo mkdir -p /var/mail
sudo chmod 775 /var/mail
sudo chgrp mail /var/mail
```

---

## 9. **Levantar los servicios**

```bash
cd ~/correo-grupo6/compose

# Construir y levantar contenedores
docker compose up -d --build

# Verificar que están corriendo
docker compose ps

# Ver logs (abre otra terminal para esto)
docker compose logs -f

# Ver contenedores
docker ps

# Ver puertos abiertos
sudo ss -ltnp | grep -E "25|587|143|993|110|995"
```

---

## 10. **Pruebas iniciales**

### 10.1 Probar SMTP con swaks
```bash
# Enviar correo de usuario1 a usuario2
swaks --to usuario1@grupo6.fpinfo.com.es \
      --from usuario2@grupo6.fpinfo.com.es \
      --server localhost \
      --header "Subject: Prueba Grupo 6" \
      --body "Hola usuario1, este es un mensaje de prueba desde Docker"

# Verificar que llegó
sudo cat /var/mail/usuario1
```

### 10.2 Probar con telnet
```bash
# Conectar al puerto SMTP
telnet localhost 25

# Comandos SMTP (escribe uno a uno)
EHLO localhost
MAIL FROM: <usuario2@grupo6.fpinfo.com.es>
RCPT TO: <usuario1@grupo6.fpinfo.com.es>
DATA
Subject: Prueba telnet
Hola desde telnet, esto es una prueba.
.
QUIT
```

### 10.3 Probar IMAP/POP3
```bash
# Probar IMAP (debe mostrar STARTTLS)
telnet localhost 143
a1 LOGIN usuario1 contraseña
a2 LIST "" "*"
a3 SELECT INBOX
a4 LOGOUT

# Probar desde el contenedor
docker exec -it dovecot-grupo6 doveadm user usuario1
```

---

## 11. **Configurar Thunderbird**

### 11.1 En tu máquina LOCAL (no en Azure)
```bash
# Instalar Thunderbird
sudo apt install -y thunderbird
```

### 11.2 Configurar cuenta en Thunderbird

**Datos de la cuenta:**
- **Nombre**: usuario1
- **Email**: usuario1@grupo6.fpinfo.com.es
- **Contraseña**: la que pusiste

**Configuración manual:**
```
Servidor de entrada (IMAP):
- Servidor: mail.grupo6.fpinfo.com.es
- Puerto: 993
- SSL: SSL/TLS
- Autenticación: Contraseña normal

Servidor de salida (SMTP):
- Servidor: mail.grupo6.fpinfo.com.es
- Puerto: 587
- SSL: STARTTLS
- Autenticación: Contraseña normal
```

### 11.3 Aceptar certificado
Cuando Thunderbird muestre la advertencia:
- ✅ "Aceptar este certificado permanentemente"
- ✅ "Confiar en esta CA"

### 11.4 Pruebas con Thunderbird
1. Envía correo de usuario1 a usuario2
2. Envía correo de usuario2 a usuario1
3. Envía un correo con archivo adjunto
4. Verifica que aparecen en la bandeja de entrada

---

## 12. **Git: Commit y push**

```bash
cd ~/correo-grupo6

# Ver estado
git status

# Añadir todos los archivos
git add .

# Hacer commit
git commit -m "Configuración completa Grupo 6: Postfix y Dovecot con Docker"

# Subir a GitHub
git push origin main

# Verificar en GitHub
gh repo view --web
```

---

## 13. **Documentación para el PDF**

Toma capturas de pantalla de:

### 13.1 Configuración
- [ ] `docker compose ps` mostrando los contenedores
- [ ] `docker ps` mostrando los contenedores corriendo
- [ ] `docker compose logs` mostrando los logs sin errores
- [ ] `sudo ss -ltnp` mostrando los puertos abiertos

### 13.2 Pruebas de envío
- [ ] Comando swaks ejecutándose
- [ ] Contenido de `/var/mail/usuario1` mostrando el correo recibido
- [ ] Prueba con telnet (pantallazo)

### 13.3 Thunderbird
- [ ] Configuración de la cuenta
- [ ] Bandeja de entrada con correos
- [ ] Envío de correo entre usuarios
- [ ] Correo con adjunto

### 13.4 GitHub
- [ ] Repositorio con los archivos subidos
- [ ] Estructura de directorios

### 13.5 DNS (si puedes)
- [ ] Comprobación de registros MX

---

## ✅ **Checklist final Grupo 6**

- [ ] SSH a Azure funcionando
- [ ] Docker y Docker Compose instalados
- [ ] Registros DNS configurados
- [ ] Estructura de directorios creada
- [ ] `main.cf` de Postfix configurado
- [ ] Certificado SSL generado
- [ ] `dovecot.conf` configurado
- [ ] `docker-compose.yml` creado
- [ ] Usuarios de prueba creados
- [ ] Contenedores levantados sin errores
- [ ] Prueba con swaks exitosa
- [ ] Prueba con telnet exitosa
- [ ] Thunderbird configurado y probado
- [ ] Git commit y push realizado
- [ ] Capturas de pantalla tomadas
- [ ] PDF generado

---

## 🆘 **Comandos útiles para solucionar problemas**

```bash
# Ver logs de un contenedor específico
docker compose logs postfix
docker compose logs dovecot

# Entrar a un contenedor
docker exec -it postfix-grupo6 bash
docker exec -it dovecot-grupo6 bash

# Ver configuración de Postfix dentro del contenedor
docker exec postfix-grupo6 postconf -n

# Reiniciar servicios
docker compose restart

# Parar todo
docker compose down

# Parar y eliminar volúmenes (borra buzones)
docker compose down -v

# Ver espacio en disco
df -h

# Ver permisos de buzones
ls -la /var/mail/
```

---

¿Tienes algún paso concreto en el que necesites ayuda? ¡Avísame!
