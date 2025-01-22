# Usa Ubuntu 20.04 como base
FROM ubuntu:20.04

# Configura variables de entorno para evitar prompts interactivos
ENV DEBIAN_FRONTEND=noninteractive

# Actualiza e instala dependencias necesarias
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y \
    curl \
    gnupg \
    software-properties-common \
    mysql-client \
    lsb-release && \
    rm -rf /var/lib/apt/lists/*

# Agrega el repositorio de Node.js 20
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get update && apt-get install -y nodejs

# Agrega el repositorio de MySQL Router
RUN curl -fsSL https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb -o mysql-apt-config.deb && \
    dpkg -i mysql-apt-config.deb && \
    apt-get update && apt-get install -y mysql-router && \
    rm -f mysql-apt-config.deb

# Crea un directorio de trabajo
WORKDIR /app

# Copia la aplicación Node.js (si tienes un archivo package.json o similar)
# COPY . .

# Instala las dependencias de la aplicación (si aplica)
# RUN npm install

# Expone el puerto 3000 para la aplicación (si es necesario)
EXPOSE 3000

# Comando por defecto (puedes cambiarlo según tu aplicación)
CMD ["node", "--version"]