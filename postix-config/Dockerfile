FROM ubuntu:24.04
ENV DEBIAN_FRONTEND=noninteractive

# Instalación de paquetes
RUN apt-get update && apt-get install -y postfix mailutils libsasl2-modules

# Configuración básica (ajusta destevez.fpinfo.com.es a tu dominio asignado)
RUN postconf -e 'myhostname = destevez.fpinfo.com.es'

# Copia de configuración local y volúmenes
COPY ./main.cf /etc/postfix/main.cf
VOLUME /var/mail

# Puertos SMTP (25) y Submission (587)
EXPOSE 25 587

# Ejecución en primer plano para que el contenedor no se detenga
CMD ["postfix", "start-fg"]
