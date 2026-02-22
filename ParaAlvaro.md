## 📋 **RESUMEN PARA ÁLVARO (compañero de Dovecot)**

### ✅ **Lo que YO (Fernando) ya he hecho:**

1. **Rama creada:** `fernandoirun/dovecot-config`
2. **Estructura de directorios:**
   ```
   dovecot/
   ├── Dockerfile
   ├── conf/
   │   ├── dovecot.conf
   │   ├── users
   │   └── conf.d/
   │       ├── 10-auth.conf
   │       ├── 10-mail.conf
   │       ├── 10-master.conf
   │       └── auth-passwdfile.conf.ext
   ├── data/
   └── compose/
       └── docker-compose-dovecot.yml
   ```

3. **Configuración implementada:**
   - Dominio: `feri.fpinto.com.es`
   - Formato buzones: **Maildir** (en `/var/mail/vhosts/%d/%n`)
   - Usuarios creados:
     - `test@feri.fpinto.com.es` / `grupo6pass`
     - `admin@feri.fpinto.com.es` / `adminpass`
     - `user1@feri.fpinto.com.es` / `pass123`
     - `user2@feri.fpinto.com.es` / `pass456`
   - Puerto IMAP: 143
   - Puerto IMAPS: 993
   - Puerto POP3: 110
   - Puerto POP3S: 995

4. **Subido a GitHub:** Todo está en la rama `fernandoirun/dovecot-config`

---

### ⚠️ **Problema actual (para que lo sepas):**

El contenedor **no arranca** porque el socket de autenticación para Postfix está activo pero Postfix no existe. En los logs aparece:
```
Error: bind(/var/spool/postfix/private/auth) failed: No such file or directory
```

**Solución:** Hay que comentar el bloque `service auth` en `dovecot.conf` (yo lo intenté pero no se aplicó correctamente).

---

### 🔧 **Lo que TIENES QUE HACER TÚ (Álvaro):**

#### Paso 1: Clonar el repositorio y cambiar a la rama
```bash
git clone https://github.com/fernandoirun/Correo-Grupo6.git
cd Correo-Grupo6
git checkout fernandoirun/dovecot-config
```

#### Paso 2: Verificar que el socket está comentado
```bash
cat dovecot/conf/dovecot.conf | grep -A5 "service auth"
```
Si ves líneas sin `#`, edita:
```bash
nano dovecot/conf/dovecot.conf
```
Comenta todo el bloque:
```conf
# service auth {
#   unix_listener /var/spool/postfix/private/auth {
#     mode = 0660
#     user = postfix
#     group = postfix
#   }
# }
```

#### Paso 3: Reconstruir el contenedor
```bash
cd compose
docker compose -f docker-compose-dovecot.yml down
docker rmi compose-dovecot:latest -f
docker compose -f docker-compose-dovecot.yml up -d --build
```

#### Paso 4: Verificar que funciona
```bash
docker ps
# Debe mostrar "Up", no "Restarting"

telnet localhost 143
# Debe conectar y mostrar bienvenida de Dovecot
```

#### Paso 5: Probar login
```bash
telnet localhost 143
a login test@feri.fpinto.com.es grupo6pass
a select INBOX
a logout
```

#### Paso 6: Hacer capturas para la práctica
```bash
docker ps
docker compose logs --tail=50
telnet localhost 143  # y capturar login exitoso
```

#### Paso 7: Subir cambios a GitHub (si los hay)
```bash
git add .
git commit -m "fix: comentado socket auth para que funcione sin Postfix"
git push origin fernandoirun/dovecot-config
```

---

### 📸 **Capturas necesarias para la práctica:**

1. `docker ps` mostrando el contenedor `Up`
2. Logs de Dovecot sin errores
3. Login exitoso en IMAP
4. Estructura de directorios
5. Contenido de `dovecot.conf`
6. Contenido de `users`

---

### 🤝 **Consejos para Álvaro:**

- Si el contenedor sigue sin arrancar, prueba:
  ```bash
  docker compose -f docker-compose-dovecot.yml logs -f
  ```
  para ver el error en tiempo real.

- Cuando Postfix esté listo, habrá que **descomentar** el socket de autenticación y asegurar que Postfix y Dovecot compartan el mismo volumen para el socket.

- Cualquier duda, lo hablamos.

---

¿Quieres que añada algo más para Álvaro?


## Capturas por Álvaro

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

