## 📋 **TODOS LOS COMANDOS QUE HE USADO EN LA PRÁCTICA**

### **1. COMANDOS DE GIT Y GITHUB**

```bash
# Verificar autenticación
gh auth status

# Clonar el repositorio
gh repo clone fernandoirun/Correo-Grupo6

# Entrar al repositorio
cd Correo-Grupo6

# Traer últimos cambios
git pull origin main

# Crear y cambiar a tu rama
git checkout -b fernandoirun/dovecot-config

# Ver rama actual
git branch

# Ver estado de los archivos
git status

# Añadir archivos al stage
git add dovecot/
git add compose/
git add documentacion-Ferir/
git add .   # (para añadir todo)

# Hacer commit
git commit -m "feat(dovecot): configuración inicial"
git commit -m "feat(dovecot): añadido usuario postfix y socket auth"
git commit -m "fix: comentado socket auth para funcionar sin Postfix"
git commit -m "docs: añadida carpeta documentacion-Ferir con README"

# Subir cambios a GitHub
git push origin fernandoirun/dovecot-config

# Ver historial de commits
git log --oneline --graph --all

# Ver repositorio en GitHub
gh repo view --web
```

---

### **2. COMANDOS PARA CREAR ESTRUCTURA DE DIRECTORIOS**

```bash
# Crear carpetas principales
mkdir -p dovecot/conf/conf.d
mkdir -p dovecot/data
mkdir -p maildata
mkdir -p compose
mkdir -p documentacion-Ferir

# Ver estructura creada
ls -la
ls -la dovecot/
ls -la dovecot/conf/
ls -la dovecot/conf/conf.d/
ls -la compose/
ls -la documentacion-Ferir/
```

---

### **3. COMANDOS PARA CREAR Y EDITAR ARCHIVOS**

```bash
# Crear/editar Dockerfile
nano dovecot/Dockerfile

# Ver contenido del Dockerfile
cat dovecot/Dockerfile

# Crear/editar configuración principal
nano dovecot/conf/dovecot.conf

# Ver contenido
cat dovecot/conf/dovecot.conf

# Crear/editar archivo de usuarios
nano dovecot/conf/users

# Ver usuarios
cat dovecot/conf/users

# Crear/editar archivos de conf.d/
nano dovecot/conf/conf.d/10-auth.conf
nano dovecot/conf/conf.d/10-mail.conf
nano dovecot/conf/conf.d/10-master.conf
nano dovecot/conf/conf.d/auth-passwdfile.conf.ext

# Ver archivos de conf.d/
cat dovecot/conf/conf.d/10-auth.conf
cat dovecot/conf/conf.d/10-mail.conf
cat dovecot/conf/conf.d/10-master.conf
cat dovecot/conf/conf.d/auth-passwdfile.conf.ext

# Crear/editar docker-compose
nano compose/docker-compose-dovecot.yml

# Ver docker-compose
cat compose/docker-compose-dovecot.yml

# Crear documentación
nano documentacion-Ferir/README.md
cat documentacion-Ferir/README.md
```

---

### **4. COMANDOS DE DOCKER Y DOCKER COMPOSE**

```bash
# Entrar al directorio compose
cd compose

# Bajar contenedor si existe
docker compose -f docker-compose-dovecot.yml down

# Construir imagen (sin caché)
docker compose -f docker-compose-dovecot.yml build --no-cache

# Levantar contenedor
docker compose -f docker-compose-dovecot.yml up -d

# Levantar contenedor reconstruyendo
docker compose -f docker-compose-dovecot.yml up -d --build

# Ver estado del contenedor
docker compose -f docker-compose-dovecot.yml ps
docker ps | grep dovecot

# Ver logs
docker compose -f docker-compose-dovecot.yml logs
docker compose -f docker-compose-dovecot.yml logs --tail=50
docker compose -f docker-compose-dovecot.yml logs -f

# Ver todos los contenedores (incluyendo detenidos)
docker ps -a | grep dovecot

# Ver información del contenedor
docker inspect dovecot-grupo6

# Ver logs de un contenedor específico
docker logs dovecot-grupo6

# Eliminar contenedor
docker rm dovecot-grupo6

# Eliminar imagen
docker rmi compose-dovecot:latest

# Limpiar Docker
docker system prune -f
docker system prune -a -f
```

---

### **5. COMANDOS DE RED Y CONEXIÓN**

```bash
# Ver puertos en escucha
netstat -tlnp | grep -E '143|993|110|995'
ss -tlnp | grep dovecot

# Probar conexión IMAP
telnet localhost 143

# Comandos dentro de telnet (para prueba IMAP)
# a login test@feri.fpinto.com.es grupo6pass
# a select INBOX
# a logout

# Probar conexión POP3
telnet localhost 110
```

---

### **6. COMANDOS PARA VER ERRORES Y DIAGNÓSTICO**

```bash
# Ver si el socket está comentado
cat dovecot/conf/dovecot.conf | grep -A10 "service auth"

# Ver logs de error
docker compose -f docker-compose-dovecot.yml logs | grep -i error

# Ver estado del contenedor
docker inspect dovecot-grupo6 | grep -A 10 -i state
```

---

### **7. COMANDOS DE DOCUMENTACIÓN (capturas)**

```bash
# Guardar salida de comandos en archivos
git branch > ~/captura_git_branch.txt
ls -la dovecot/ > ~/captura_estructura.txt
cat dovecot/Dockerfile > ~/captura_dockerfile.txt
cat dovecot/conf/dovecot.conf > ~/captura_dovecot_conf.txt
cat dovecot/conf/users > ~/captura_users.txt
cat compose/docker-compose-dovecot.yml > ~/captura_compose.txt
docker compose -f compose/docker-compose-dovecot.yml ps > ~/captura_docker_ps.txt
```

---

### **8. COMANDOS PARA SUBIR CAMBIOS (resumen final)**

```bash
# Subir todo a GitHub
cd /home/feri/Correo-Grupo6
git status
git add .
git commit -m "feat: configuración completa de Dovecot"
git push origin fernandoirun/dovecot-config
```

---

## 📝 **RESUMEN PARA LA DOCUMENTACIÓN**

Puedes poner este bloque en tu documentación:

> *"Para el desarrollo de la práctica he utilizado los siguientes comandos organizados por categorías: comandos de Git/GitHub para la gestión del repositorio y ramas, comandos de shell para crear la estructura de directorios, edición de archivos con nano, comandos de Docker y Docker Compose para la construcción y gestión del contenedor de Dovecot, y comandos de red como telnet y netstat para verificar la conectividad de los puertos IMAP/POP3."*
