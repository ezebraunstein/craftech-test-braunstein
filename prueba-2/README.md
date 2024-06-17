# Explicación Deployment Docker y docker-compose

El código presente en el repositorio de prueba requiere construir por separado los contenedores del frontend y el backend. Cada subdirectorio cuenta con un archivo Dockerfile donde se establece la imagen base a utilizar, se define el directorio de trabajo y las variables de entorno, se instalan las dependencias necesarias, etc.

Además, cada subdirectorio cuenta con su propio archivo docker-compose.yml, donde se define cómo construir, configurar y exponer cada contenedor. Esto requiere moverse entre los diferentes subdirectorios y construir dos veces los distintos contenedores.

Para realizar la prueba-2 fue necesario unificar los dos archivos docker-compose.yml en uno solo. De esta forma, moviéndolo al directorio raíz, se pueden construir todos los contenedores con un solo comando (docker-compose up --build).

# Despliegue local

1. Instalar Docker y Docker Compose

2. Es necesario crear el archivo .env en el directorio ./backend y cargar los siguientes datos. Deben coincidir con los datos de .env.postgres:
SQL_ENGINE, 
SQL_DATABASE, 
SQL_USER, 
SQL_PASSWORD, 
SQL_HOST, 
SQL_PORT

3. Descomentar la constante API_SERVER localhost:3000, y comentar la IPv4 de EC2 en el archivo constant.js:
./prueba-2/frontend/src/config/constant.js
export const API_SERVER = 'http://localhost:8000/api/'; <--- API_SERVER tiene que apuntar a localhost puerto 8000

4. Moverse al directorio donde se encuntra el archivo docker-compose.yml y ejecutar la build:
docker-compose up --build

5. Acceder al frontend mediante http://localhost:3000

# Despliegue en AWS EC2

# ACTUALMENTE LA APP (vPRUEBA-2) ESTÁ DEPLOYADA EN http://54.82.112.38:3000

1. Crear una instancia EC2 en AWS (Amazon Linux 2023 AMI, 64-bit (x86), t2.small, permitir tráfico SSH y HTTP)

2. Conectarse a la instancia mediante SSH usando la terminal y la key file, o mediante EC2 Instance Connect

3. Instalar Docker, Docker Compose y Git en la instancia:
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

5. Es necesario crear el archivo .env en el directorio ./backend y cargar los siguientes datos. Deben coincidir con los datos de .env.postgres:
SQL_ENGINE, 
SQL_DATABASE, 
SQL_USER, 
SQL_PASSWORD, 
SQL_HOST, 
SQL_PORT

6. Comentar la constante API_SERVER localhost:3000 y agregar la IPv4 de la instancia de EC2 en el archivo constant.js:
./prueba-2/frontend/src/config/constant.js
export const API_SERVER = 'http://XXX.X.X.X:8000/api/'; <--- API_SERVER tiene que apuntar a la IPv4 de EC2 puerto 8000

7. Agregar la IPv4 pública de EC2 a los CORS ALLOWED ORIGINS en el archivo ./prueba-2/backend/core/settings.py
CORS_ALLOWED_ORIGINS = [
"http://localhost:3000", 
"http://127.0.0.1:3000", 
"http://XXX.X.X.X:3000", <--- IPv4 pública de EC2
]

8. Asegurarse de que los puertos '3000' y '8000' estén abiertos en el grupo de seguridad de EC2 (Inbound Rules).

9. Moverse al directorio donde se encuentra el archivo docker-compose.yml y ejecutar la build
docker-compose up --build

10. Acceder al frontend mediante http://XXX.X.X.X:3000 (IPv4 pública de EC2)




