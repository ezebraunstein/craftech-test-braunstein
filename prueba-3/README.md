# Despliegue en AWS EC2 + CI/CD GitHub Actions

# ACTUALMENTE LA APP (vPRUEBA-3) ESTÁ DEPLOYADA EN http://54.89.29.237

1. Crear una instancia EC2 en AWS (Amazon Linux 2023 AMI, 64-bit (x86), t2.small, permitir tráfico SSH y HTTP)

2. Conectarse a la instancia mediante SSH usando la terminal y la key file, o mediante EC2 Instance Connect

3. Instalar Docker, Docker Compose y Git en la instancia.
    sudo yum update -y
    sudo yum install docker -y
    sudo service docker start
    sudo usermod -a -G docker ec2-user
    sudo usermod -aG docker $USER
    newgrp docker
    sudo curl -L "https://github.com/docker/compose/releases/download/v2.27.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo yum install git -y

4. Clonar el repositorio de GIT en la instancia
    git clone https://github.com/ezebraunstein/craftech-test-braunstein

5. Agregar los repository secrets al repositorio de Github
    DOCKERHUB_PASSWORD
    DOCKERHUB_USERNAME
    EC2_PUBLIC_DNS
    SSH_PRIVATE_KEY

6. Es necesario crear el archivo .env en el directorio ./backend y cargar los siguientes datos. Deben coincidir con los datos de .env.postgres
    SQL_ENGINE,
    SQL_DATABASE, 
    SQL_USER, 
    SQL_PASSWORD, 
    SQL_HOST, 
    SQL_PORT

7. Agregar la IPv4 de la instancia de EC2 en el archivo constant.js
    ./prueba-2/frontend/src/config/constant.js
    export const API_SERVER = 'http://XXX.X.X.X:8000/api/'; <--- API_SERVER tiene que apuntar a la IPv4 de EC2 puerto 8000

8. Agregar la IPv4 pública de EC2 a los CORS ALLOWED ORIGINS en el archivo ./prueba-3/backend/core/settings.py
    CORS_ALLOWED_ORIGINS = [
    "http://XXX.X.X.X:8000",
    "http://XXX.X.X.X:80",
    "http://XXX.X.X.X",
    ]

9. Asegurarse de que el puerto '8000' esté abierto en el grupo de seguridad de EC2 (Inbound Rules).

10. Una vez realizado algún cambio en el archivo index.html, luego de pushearlo, automáticamente comenzará el workflow CI/CD

11. Acceder al frontend mediante http://XXX.X.X.X (IPv4 pública de EC2)



