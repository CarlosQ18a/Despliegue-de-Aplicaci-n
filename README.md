# Informe del Proyecto: Despliegue de Aplicación Frontend con Backend Simulado usando Docker

## 1. Título  
**Despliegue y prueba de una aplicación frontend React conectada a un backend simulado (mockAPI) utilizando Docker y Docker Compose**

## 2. Fundamentos

Este proyecto simula un entorno moderno de desarrollo donde frontend y backend se despliegan en contenedores Docker. El backend simulado usa json-server para crear una API REST a partir de un JSON. El frontend React se sirve con NGINX.

Conceptos aplicados:
- Contenerización de frontend con Docker.
- Simulación de backend REST con json-server.
- Comunicación entre contenedores mediante Docker Compose.
- Manejo de puertos y volúmenes para persistencia y conectividad.
- Solución de problemas comunes (puertos, CORS).
- Uso de GitHub para control de versiones y clonación.

## 3. Conocimientos Previos Requeridos
- Conceptos básicos de Docker (imágenes, contenedores, volúmenes).
- Uso de Git y línea de comandos.
- Node.js, npm y desarrollo React.
- Entendimiento de APIs REST.

## 4. Objetivos del Proyecto
- Clonar y ejecutar un backend simulado con json-server.
- Ejecutar frontend React en modo desarrollo.
- Crear un Dockerfile para contenerizar el frontend.
- Construir y ejecutar la imagen Docker del frontend.
- Verificar que el frontend consume correctamente el backend simulado.

## 5. Equipos y Herramientas Necesarias
- PC con Docker y Docker Compose instalados.
- Git.
- Node.js y npm.
- Editor de código (VS Code recomendado).
- Navegador web moderno.

## 6. Material de apoyo
- [Repositorio Backend MockAPI](https://github.com/Daviddotcoms/mockAPI)  
- [Documentación Docker](https://docs.docker.com/)  
- [json-server](https://github.com/typicode/json-server)  

## 7. Procedimiento

### Paso 1: Clonar y ejecutar backend simulado
```bash
git clone https://github.com/Daviddotcoms/mockAPI.git
cd mockAPI
npm install
npm start
```
Backend disponible en: http://localhost:3100

Endpoints:
- /classmates
- /teachers
### Paso 2: Clonar y ejecutar frontend en modo desarrollo
Clonar el repositorio del frontend y ejecutar:
```bash
npm install
npm run dev
```
Verificar que accede a http://localhost:3100.

### Paso 3: Dockerfile para el frontend
# Construcción en dos etapas para optimizar imagen final

# Etapa 1: Build React app
```bash
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
```
# Etapa 2: Servir con NGINX
```bash
FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Paso 4:  Construir imagen Docker
```bash
docker build -t suda-frontend .
```
### Paso 5: Ejecutar contenedor del frontend
```bash
docker run -d -p 3000:80 suda-frontend
```
Se usa puerto 3000 para evitar conflicto con 8080.
### Paso 6: Verificar comunicación frontend-backend

Acceder a http://localhost:3000 y confirmar que se muestran datos del backend simulado.
## 8. Resultados Obtenidos
- Backend estable en http://localhost:3100.
- Frontend desplegado correctamente en Docker en http://localhost:3000.
- Comunicación exitosa entre servicios.
- Conflicto de puertos resuelto cambiando puerto a 3000.
## 9. Bibliografía
- Docker, Inc. (2024). Documentación oficial de Docker. https://docs.docker.com/
- Typicode. (2024). json-server: Fake REST API. https://github.com/typicode/json-server
- ReactJS. (2024). Documentación oficial de React. https://react.dev/
- Node.js Foundation. (2024). Documentación oficial de Node.js. https://nodejs.org/en/docs
- GitHub, Inc. (2024). Repositorio mockAPI. https://github.com/Daviddotcoms/mockAPI
- NGINX. (2024). Using NGINX for serving static files. https://docs.nginx.com/

