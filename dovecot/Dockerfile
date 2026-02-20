FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    dovecot-core \
    dovecot-imapd \
    dovecot-pop3d \
    dovecot-lmtpd \
    && rm -rf /var/lib/apt/lists/*

# Crear usuario virtual para los buzones
RUN groupadd -g 5000 vmail && \
    useradd -u 5000 -g vmail -s /usr/sbin/nologin -d /var/mail vmail

# Crear directorio para buzones con el dominio del grupo 6
RUN mkdir -p /var/mail/vhosts/feri.fpinto.com.es && \
    chown -R vmail:vmail /var/mail

VOLUME /var/mail

EXPOSE 143 993 110 995

CMD ["dovecot", "-F"]
