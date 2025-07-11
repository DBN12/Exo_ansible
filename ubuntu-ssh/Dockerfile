# Image de base Ubuntu 24.04
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Installation des paquets
RUN apt-get update && apt-get install -y \
    openssh-server \
    sudo \
    iproute2 \
    iputils-ping \
    net-tools \
    dnsutils \
    curl \
    wget \
    traceroute \
    telnet \
    lsof \
    tcpdump \
    htop \
    strace \
    vim \
    less \
    nano \
    gnupg \
    && rm -rf /var/lib/apt/lists/* && \
    apt-get clean

# Préparer le répertoire nécessaire à sshd
RUN mkdir /var/run/sshd

# Définir un mot de passe pour root (à ne jamais faire en production)
RUN echo "root:P@ssw0rd" | chpasswd

# Autoriser la connexion root par mot de passe
RUN sed -i 's/^#\?PermitRootLogin .*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?PasswordAuthentication .*/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Fix PAM (évite que SSH déconnecte l'utilisateur immédiatement)
RUN sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd

# Expose port SSH
EXPOSE 22

# Ajouter la clé publique d’Ansible dans authorized_keys
RUN mkdir -p /root/.ssh
COPY authorized_keys /root/.ssh/authorized_keys
RUN chmod 600 /root/.ssh/authorized_keys

# Lancer SSH en avant-plan pour maintenir le conteneur actif
CMD ["/usr/sbin/sshd", "-D"]
