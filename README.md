# docker-compose

# 🚀 Configuración y Ejecución de un Servicio con Docker Compose

Este documento explica cómo instalar, configurar y ejecutar un servicio usando **Docker Compose**, desde cero hasta que esté completamente operativo.

---

## 1️⃣ **Requisitos Previos**  
Antes de comenzar, asegúrate de tener instalado:  
- **Docker** → [Descargar Docker](https://docs.docker.com/get-docker/)  
- **Docker Compose** → [Instalar Docker Compose](https://docs.docker.com/compose/install/)  

Para verificar la instalación, ejecuta:  
```sh
docker --version
docker-compose --version
2️⃣ Crear el Archivo docker-compose.yml
Dentro de tu proyecto, crea un archivo llamado docker-compose.yml.
Aquí un ejemplo básico para levantar una aplicación en Node.js con MongoDB:

yaml
Copiar
Editar
version: '3.8'

services:
  app:
    image: node:18
    container_name: mi-app
    working_dir: /usr/src/app
    volumes:
      - .:/usr/src/app
    ports:
      - "3000:3000"
    depends_on:
      - mongo
    command: "npm start"

  mongo:
    image: mongo:latest
    container_name: mi-mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db

volumes:
  mongo_data:
En este archivo:

Se define un servicio app basado en la imagen de Node.js.
Se usa un volumen para sincronizar archivos entre el contenedor y el host.
Se expone el puerto 3000.
Se define otro servicio mongo basado en MongoDB.
Se crea un volumen persistente mongo_data para guardar datos de la base.
3️⃣ Construir y Levantar los Contenedores
Para iniciar el servicio, ejecuta en la carpeta donde está el archivo docker-compose.yml:

sh
Copiar
Editar
docker-compose up -d
Esto hará lo siguiente:
✅ Descargará las imágenes necesarias si no están disponibles.
✅ Creará los contenedores y los ejecutará en segundo plano.

Para ver los contenedores en ejecución:

sh
Copiar
Editar
docker ps
4️⃣ Verificar que Todo Funciona
Si necesitas revisar los logs de los contenedores, usa:

sh
Copiar
Editar
docker-compose logs -f
Si quieres entrar dentro del contenedor de MongoDB para inspeccionar la base de datos:

sh
Copiar
Editar
docker exec -it mi-mongo mongosh
Si necesitas acceder al contenedor de la aplicación:

sh
Copiar
Editar
docker exec -it mi-app sh
5️⃣ Detener y Eliminar los Contenedores
Si deseas detener y eliminar todos los contenedores, ejecuta:

sh
Copiar
Editar
docker-compose down
Esto apagará los servicios y eliminará los contenedores, pero mantendrá los volúmenes de datos.

Si también deseas eliminar los volúmenes y redes asociadas, usa:

sh
Copiar
Editar
docker-compose down -v
6️⃣ Reiniciar con Cambios
Si realizaste modificaciones en los archivos de configuración o en el código de la aplicación y necesitas reconstruir los contenedores, usa:

sh
Copiar
Editar
docker-compose up --build -d
Esto volverá a construir las imágenes y reiniciará los servicios con los cambios aplicados.

7️⃣ Eliminar Contenedores y Volúmenes Manualmente
Si necesitas eliminar todos los contenedores creados con Docker Compose:

sh
Copiar
Editar
docker rm -f $(docker ps -aq)
Para eliminar todas las imágenes de Docker:

sh
Copiar
Editar
docker rmi -f $(docker images -aq)
Para limpiar todos los volúmenes:

sh
Copiar
Editar
docker volume rm $(docker volume ls -q)
8️⃣ Ejemplo con Variables de Entorno (.env)
Si deseas gestionar credenciales o configuraciones de forma segura, usa un archivo .env.
Ejemplo de .env:

ini
Copiar
Editar
MONGO_USER=admin
MONGO_PASSWORD=secret123
Luego, en docker-compose.yml, usa:

yaml
Copiar
Editar
environment:
  MONGO_INITDB_ROOT_USERNAME: ${MONGO_USER}
  MONGO_INITDB_ROOT_PASSWORD: ${MONGO_PASSWORD}
Esto evita almacenar datos sensibles en el archivo docker-compose.yml.

