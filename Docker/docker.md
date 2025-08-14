# Guía Completa de Docker - Comandos Esenciales

## 📋 Tabla de Contenidos
- [Instalación y Configuración](#instalación-y-configuración)
- [Comandos Básicos](#comandos-básicos)
- [Gestión de Imágenes](#gestión-de-imágenes)
- [Gestión de Contenedores](#gestión-de-contenedores)
- [Docker Build y Dockerfile](#docker-build-y-dockerfile)
- [Volúmenes y Almacenamiento](#volúmenes-y-almacenamiento)
- [Redes en Docker](#redes-en-docker)
- [Docker Compose](#docker-compose)
- [Registry y Docker Hub](#registry-y-docker-hub)
- [Logs y Monitoreo](#logs-y-monitoreo)
- [Optimización y Limpieza](#optimización-y-limpieza)
- [Docker Multi-stage](#docker-multi-stage)
- [Seguridad](#seguridad)
- [Comandos Avanzados](#comandos-avanzados)
- [Troubleshooting](#troubleshooting)

---

## ⚙️ Instalación y Configuración

### Verificar instalación
```bash
# Verificar versión de Docker
docker --version

# Ver información detallada del sistema Docker
docker version

# Ver información del sistema Docker
docker info

# Verificar que Docker está corriendo
docker run hello-world

# Ver configuración de Docker
docker system info
```

### Configuración inicial
```bash
# Agregar usuario al grupo docker (Linux)
sudo usermod -aG docker $USER

# Reiniciar sesión o usar:
newgrp docker

# Configurar Docker para iniciar automáticamente
sudo systemctl enable docker

# Iniciar servicio Docker
sudo systemctl start docker

# Ver estado del servicio
sudo systemctl status docker
```

---

## 🚀 Comandos Básicos

### Información y ayuda
```bash
# Ayuda general
docker --help

# Ayuda de comando específico
docker run --help

# Ver versión detallada
docker version

# Información del daemon Docker
docker info

# Listar todos los comandos disponibles
docker help
```

### Comandos de sistema
```bash
# Ver uso de disco por Docker
docker system df

# Limpiar sistema completo
docker system prune

# Limpiar sistema incluyendo volúmenes
docker system prune -a --volumes

# Ver eventos en tiempo real
docker system events

# Ver procesos Docker
docker system info
```

---

## 🖼️ Gestión de Imágenes

### Descargar y listar imágenes
```bash
# Descargar imagen desde Docker Hub
docker pull nginx

# Descargar versión específica
docker pull nginx:1.21

# Descargar imagen de registry específico
docker pull registry.gitlab.com/user/project:tag

# Listar todas las imágenes locales
docker images

# Listar imágenes con formato específico
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

# Buscar imágenes en Docker Hub
docker search nginx

# Ver historial de imagen
docker history nginx

# Inspeccionar imagen detalladamente
docker inspect nginx
```

### Crear y gestionar imágenes
```bash
# Construir imagen desde Dockerfile
docker build -t mi-app .

# Construir con contexto específico
docker build -t mi-app -f Dockerfile.prod .

# Construir con argumentos
docker build -t mi-app --build-arg NODE_VERSION=16 .

# Construir sin cache
docker build --no-cache -t mi-app .

# Etiquetar imagen existente
docker tag mi-app:latest mi-app:v1.0

# Crear imagen desde contenedor
docker commit contenedor-id nueva-imagen:tag

# Guardar imagen en archivo tar
docker save -o mi-app.tar mi-app:latest

# Cargar imagen desde archivo tar
docker load -i mi-app.tar

# Exportar imagen comprimida
docker save mi-app:latest | gzip > mi-app.tar.gz
```

### Eliminar imágenes
```bash
# Eliminar imagen específica
docker rmi imagen:tag

# Eliminar imagen por ID
docker rmi abc123def456

# Eliminar múltiples imágenes
docker rmi imagen1:tag imagen2:tag

# Forzar eliminación
docker rmi -f imagen:tag

# Eliminar imágenes sin etiquetar (dangling)
docker image prune

# Eliminar todas las imágenes no utilizadas
docker image prune -a

# Eliminar imágenes por filtro
docker images -f "dangling=true" -q | xargs docker rmi
```

---

## 📦 Gestión de Contenedores

### Crear y ejecutar contenedores
```bash
# Ejecutar contenedor básico
docker run nginx

# Ejecutar en background (detached)
docker run -d nginx

# Ejecutar con nombre personalizado
docker run --name mi-nginx -d nginx

# Ejecutar con puerto mapeado
docker run -p 8080:80 -d nginx

# Ejecutar con variables de entorno
docker run -e NODE_ENV=production -d mi-app

# Ejecutar con volumen montado
docker run -v /host/path:/container/path -d nginx

# Ejecutar modo interactivo
docker run -it ubuntu bash

# Ejecutar con límites de recursos
docker run --memory="512m" --cpus="1.5" -d mi-app

# Ejecutar y eliminar al salir
docker run --rm -it ubuntu bash

# Ejecutar con usuario específico
docker run --user 1000:1000 -d mi-app

# Ejecutar con restart policy
docker run --restart=unless-stopped -d nginx
```

### Gestionar contenedores en ejecución
```bash
# Listar contenedores ejecutándose
docker ps

# Listar todos los contenedores (incluye detenidos)
docker ps -a

# Ver solo IDs de contenedores
docker ps -q

# Filtrar contenedores por estado
docker ps -f "status=running"

# Ver últimos n contenedores creados
docker ps -n 5

# Formatear salida personalizada
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Ver procesos dentro de contenedor
docker top contenedor-nombre

# Ver estadísticas en tiempo real
docker stats

# Ver estadísticas de contenedor específico
docker stats contenedor-nombre
```

### Controlar contenedores
```bash
# Detener contenedor
docker stop contenedor-nombre

# Detener forzadamente
docker kill contenedor-nombre

# Iniciar contenedor detenido
docker start contenedor-nombre

# Reiniciar contenedor
docker restart contenedor-nombre

# Pausar contenedor
docker pause contenedor-nombre

# Reanudar contenedor pausado
docker unpause contenedor-nombre

# Eliminar contenedor
docker rm contenedor-nombre

# Eliminar contenedor en ejecución (forzar)
docker rm -f contenedor-nombre

# Eliminar todos los contenedores detenidos
docker container prune
```

### Interactuar con contenedores
```bash
# Ejecutar comando en contenedor corriendo
docker exec contenedor-nombre ls -la

# Acceso interactivo a contenedor
docker exec -it contenedor-nombre bash

# Ejecutar como usuario específico
docker exec -u root -it contenedor-nombre bash

# Copiar archivo desde contenedor
docker cp contenedor:/ruta/archivo.txt ./archivo.txt

# Copiar archivo a contenedor
docker cp archivo.txt contenedor:/ruta/

# Inspeccionar contenedor
docker inspect contenedor-nombre

# Ver cambios en filesystem del contenedor
docker diff contenedor-nombre

# Obtener IP del contenedor
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' contenedor-nombre
```

---

## 🏗️ Docker Build y Dockerfile

### Comandos de build
```bash
# Build básico
docker build -t mi-app:latest .

# Build con nombre y etiqueta
docker build -t mi-usuario/mi-app:v1.0 .

# Build desde Dockerfile específico
docker build -f Dockerfile.prod -t mi-app:prod .

# Build con argumentos
docker build --build-arg VERSION=1.0 -t mi-app .

# Build multi-plataforma
docker buildx build --platform linux/amd64,linux/arm64 -t mi-app .

# Build sin cache
docker build --no-cache -t mi-app .

# Build con cache desde imagen
docker build --cache-from mi-app:latest -t mi-app:v2 .

# Build con contexto remoto
docker build -t mi-app https://github.com/user/repo.git

# Build con squash (combinar layers)
docker build --squash -t mi-app .

# Ver pasos de build detalladamente
docker build --progress=plain -t mi-app .
```

### Dockerfile básico ejemplo
```dockerfile
# Dockerfile de ejemplo
FROM node:16-alpine

# Metadata
LABEL maintainer="tu-email@example.com"
LABEL version="1.0"

# Argumentos de build
ARG NODE_ENV=production

# Variables de entorno
ENV NODE_ENV=${NODE_ENV}
ENV PORT=3000

# Directorio de trabajo
WORKDIR /app

# Copiar archivos de dependencias
COPY package*.json ./

# Instalar dependencias
RUN npm ci --only=production && npm cache clean --force

# Crear usuario no-root
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001

# Copiar código fuente
COPY . .

# Cambiar permisos
RUN chown -R nextjs:nodejs /app

# Cambiar a usuario no-root
USER nextjs

# Exponer puerto
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:3000/health || exit 1

# Comando por defecto
CMD ["npm", "start"]
```

### .dockerignore ejemplo
```gitignore
# .dockerignore
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
.nyc_output
coverage
.nyc_output
.coverage
.coverage.*
.env.local
.env.development.local
.env.test.local
.env.production.local
```

---

## 💾 Volúmenes y Almacenamiento

### Tipos de volúmenes
```bash
# Crear volumen nombrado
docker volume create mi-volumen

# Listar volúmenes
docker volume ls

# Inspeccionar volumen
docker volume inspect mi-volumen

# Eliminar volumen
docker volume rm mi-volumen

# Eliminar volúmenes no utilizados
docker volume prune

# Crear volumen con driver específico
docker volume create --driver local mi-volumen
```

### Usar volúmenes en contenedores
```bash
# Montar volumen nombrado
docker run -v mi-volumen:/app/data -d mi-app

# Montar directorio del host (bind mount)
docker run -v /host/path:/container/path -d mi-app

# Montar con permisos específicos
docker run -v /host/path:/container/path:ro -d mi-app

# Montar volumen temporal (tmpfs)
docker run --tmpfs /tmp -d mi-app

# Múltiples volúmenes
docker run -v vol1:/data1 -v vol2:/data2 -d mi-app

# Ver volúmenes de contenedor
docker inspect contenedor | grep -A 10 "Mounts"

# Crear volumen con opciones específicas
docker volume create --opt type=nfs --opt o=addr=192.168.1.1 mi-nfs-vol
```

### Backup y restore de volúmenes
```bash
# Backup de volumen
docker run --rm -v mi-volumen:/backup-vol -v $(pwd):/backup ubuntu tar czf /backup/backup.tar.gz -C /backup-vol .

# Restore de volumen
docker run --rm -v mi-volumen:/backup-vol -v $(pwd):/backup ubuntu tar xzf /backup/backup.tar.gz -C /backup-vol

# Copiar datos entre volúmenes
docker run --rm -v vol-origen:/from -v vol-destino:/to alpine sh -c "cp -r /from/* /to/"

# Limpiar volumen
docker run --rm -v mi-volumen:/vol alpine sh -c "rm -rf /vol/*"
```

---

## 🌐 Redes en Docker

### Gestión de redes
```bash
# Listar redes
docker network ls

# Crear red personalizada
docker network create mi-red

# Crear red con subnet específico
docker network create --subnet=172.18.0.0/16 mi-red

# Crear red bridge personalizada
docker network create --driver bridge mi-bridge

# Inspeccionar red
docker network inspect mi-red

# Eliminar red
docker network rm mi-red

# Eliminar redes no utilizadas
docker network prune

# Conectar contenedor a red
docker network connect mi-red contenedor-nombre

# Desconectar contenedor de red
docker network disconnect mi-red contenedor-nombre
```

### Usar redes en contenedores
```bash
# Ejecutar contenedor en red específica
docker run --network mi-red -d nginx

# Ejecutar con alias en la red
docker run --network mi-red --network-alias web -d nginx

# Publicar puerto en IP específica
docker run -p 127.0.0.1:8080:80 -d nginx

# Publicar en puerto aleatorio
docker run -P -d nginx

# Sin red (solo loopback)
docker run --network none -d mi-app

# Usar red del host
docker run --network host -d mi-app

# Usar red de otro contenedor
docker run --network container:contenedor1 -d mi-app
```

### Ejemplos de comunicación entre contenedores
```bash
# Crear red personalizada
docker network create app-network

# Ejecutar base de datos
docker run --name db --network app-network -e MYSQL_ROOT_PASSWORD=secret -d mysql:8

# Ejecutar aplicación conectada a BD
docker run --name app --network app-network -e DB_HOST=db -d mi-app

# Test de conectividad
docker exec app ping db

# Ver IPs en la red
docker network inspect app-network
```

---

## 🐙 Docker Compose

### Comandos básicos de Compose
```bash
# Iniciar servicios
docker-compose up

# Iniciar en background
docker-compose up -d

# Construir servicios antes de iniciar
docker-compose up --build

# Iniciar servicios específicos
docker-compose up web db

# Ver logs
docker-compose logs

# Ver logs de servicio específico
docker-compose logs web

# Seguir logs en tiempo real
docker-compose logs -f

# Detener servicios
docker-compose stop

# Detener y eliminar contenedores
docker-compose down

# Detener y eliminar todo (incluye volúmenes)
docker-compose down -v

# Reiniciar servicios
docker-compose restart

# Ver servicios corriendo
docker-compose ps
```

### Gestión de servicios
```bash
# Ejecutar comando en servicio
docker-compose exec web bash

# Ejecutar comando sin TTY
docker-compose exec -T web ls

# Escalar servicio
docker-compose up --scale web=3

# Ver configuración final
docker-compose config

# Validar archivo compose
docker-compose config --quiet

# Construir imágenes
docker-compose build

# Construir sin cache
docker-compose build --no-cache

# Pull de imágenes
docker-compose pull

# Pausar servicios
docker-compose pause

# Reanudar servicios
docker-compose unpause
```

### docker-compose.yml ejemplo
```yaml
version: '3.8'

services:
  # Servicio web
  web:
    build: 
      context: .
      dockerfile: Dockerfile
      args:
        NODE_VERSION: 16
    container_name: mi-app-web
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    volumes:
      - ./src:/app/src
      - node_modules:/app/node_modules
    depends_on:
      - db
      - redis
    networks:
      - app-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000"]
      interval: 30s
      timeout: 10s
      retries: 5
    deploy:
      resources:
        limits:
          memory: 512M
        reservations:
          memory: 256M

  # Base de datos
  db:
    image: postgres:13
    container_name: mi-app-db
    environment:
      POSTGRES_DB: miapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - "5432:5432"
    networks:
      - app-network
    restart: unless-stopped

  # Cache Redis
  redis:
    image: redis:6-alpine
    container_name: mi-app-redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - app-network
    restart: unless-stopped
    command: redis-server --appendonly yes

  # Reverse Proxy
  nginx:
    image: nginx:alpine
    container_name: mi-app-nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - web
    networks:
      - app-network
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
  node_modules:

networks:
  app-network:
    driver: bridge
```

### Docker Compose override
```bash
# Usar archivo override específico
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up

# Variables de entorno
docker-compose --env-file .env.prod up

# Proyectos con nombre personalizado
docker-compose -p mi-proyecto up
```

---

## 🏪 Registry y Docker Hub

### Docker Hub
```bash
# Login a Docker Hub
docker login

# Login con usuario específico
docker login -u mi-usuario

# Logout
docker logout

# Push imagen a Docker Hub
docker push mi-usuario/mi-app:latest

# Push todas las tags
docker push mi-usuario/mi-app --all-tags

# Tag imagen para push
docker tag mi-app:latest mi-usuario/mi-app:v1.0

# Pull imagen privada
docker pull mi-usuario/mi-app-privada:latest
```

### Registry privado
```bash
# Login a registry privado
docker login registry.empresa.com

# Tag para registry privado
docker tag mi-app:latest registry.empresa.com/proyecto/mi-app:latest

# Push a registry privado
docker push registry.empresa.com/proyecto/mi-app:latest

# Pull desde registry privado
docker pull registry.empresa.com/proyecto/mi-app:latest

# Ejecutar registry local
docker run -d -p 5000:5000 --name registry registry:2

# Push a registry local
docker tag mi-app:latest localhost:5000/mi-app:latest
docker push localhost:5000/mi-app:latest
```

### Gestión de imágenes en registry
```bash
# Ver tags disponibles (usando API)
curl -X GET https://registry.hub.docker.com/v2/repositories/library/nginx/tags/

# Ver información de imagen
docker manifest inspect nginx:latest

# Eliminar imagen de registry (si soportado)
docker rmi registry.empresa.com/proyecto/mi-app:tag
```

---

## 📊 Logs y Monitoreo

### Logs de contenedores
```bash
# Ver logs de contenedor
docker logs contenedor-nombre

# Seguir logs en tiempo real
docker logs -f contenedor-nombre

# Ver últimas n líneas
docker logs --tail 50 contenedor-nombre

# Ver logs con timestamps
docker logs -t contenedor-nombre

# Ver logs desde fecha específica
docker logs --since "2023-01-01" contenedor-nombre

# Ver logs hasta fecha específica
docker logs --until "2023-12-31" contenedor-nombre

# Ver logs de múltiples contenedores
docker logs contenedor1 & docker logs contenedor2

# Configurar driver de logs
docker run --log-driver json-file --log-opt max-size=10m --log-opt max-file=3 -d nginx
```

### Monitoreo y estadísticas
```bash
# Ver estadísticas en tiempo real
docker stats

# Estadísticas de contenedor específico
docker stats contenedor-nombre

# Estadísticas sin streaming
docker stats --no-stream

# Formatear salida de estadísticas
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Ver procesos de contenedor
docker top contenedor-nombre

# Ver eventos del daemon
docker events

# Filtrar eventos
docker events --filter container=contenedor-nombre

# Ver eventos desde fecha
docker events --since "2023-01-01"
```

### Health checks
```bash
# Verificar health status
docker inspect --format='{{.State.Health.Status}}' contenedor-nombre

# Ver historial de health checks
docker inspect --format='{{range .State.Health.Log}}{{.Output}}{{end}}' contenedor-nombre

# Ejecutar health check manualmente
docker exec contenedor-nombre curl -f http://localhost:8080/health
```

---

## 🧹 Optimización y Limpieza

### Comandos de limpieza
```bash
# Limpiar todo lo no utilizado
docker system prune

# Limpiar incluyendo imágenes sin usar
docker system prune -a

# Limpiar incluyendo volúmenes
docker system prune --volumes

# Limpiar sin confirmación
docker system prune -f

# Ver espacio usado
docker system df

# Ver espacio detallado
docker system df -v

# Limpiar contenedores detenidos
docker container prune

# Limpiar imágenes sin etiquetar
docker image prune

# Limpiar todas las imágenes no utilizadas
docker image prune -a

# Limpiar volúmenes no utilizados
docker volume prune

# Limpiar redes no utilizadas
docker network prune
```

### Optimización de imágenes
```bash
# Ver layers de imagen
docker history mi-app:latest

# Analizar tamaño por layer
docker history --no-trunc mi-app:latest

# Usar dive para análizar imagen (herramienta externa)
dive mi-app:latest

# Comprimir imagen
docker save mi-app:latest | gzip > mi-app.tar.gz

# Crear imagen más pequeña con multi-stage
docker build -f Dockerfile.multistage -t mi-app:small .
```

### Limpieza automatizada
```bash
# Script de limpieza
#!/bin/bash
echo "Limpiando contenedores detenidos..."
docker container prune -f

echo "Limpiando imágenes sin usar..."
docker image prune -a -f

echo "Limpiando volúmenes no utilizados..."
docker volume prune -f

echo "Limpiando redes no utilizadas..."
docker network prune -f

echo "Limpieza completada!"
docker system df
```

---

## 🏗️ Docker Multi-stage

### Ejemplo básico multi-stage
```dockerfile
# Multi-stage Dockerfile para aplicación Node.js
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:16-alpine AS runtime
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .
USER nextjs
EXPOSE 3000
CMD ["npm", "start"]
```

### Multi-stage avanzado
```dockerfile
# Multi-stage con múltiples stages
FROM golang:1.18 AS dependencies
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

FROM dependencies AS build
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

FROM dependencies AS test
RUN go test -v ./...

FROM alpine:latest AS runtime
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=build /app/main .
CMD ["./main"]
```

### Comandos para multi-stage
```bash
# Build hasta stage específico
docker build --target test -t mi-app:test .

# Build multiple targets
docker build --target runtime -t mi-app:latest .
docker build --target test -t mi-app:test .

# Ver stages disponibles
docker build --target help .
```

---

## 🔒 Seguridad

### Usuarios y permisos
```bash
# Ejecutar como usuario específico
docker run --user 1000:1000 -d mi-app

# Crear usuario en Dockerfile
# RUN addgroup -g 1001 -S appgroup && \
#     adduser -S appuser -u 1001 -G appgroup

# Verificar usuario del contenedor
docker exec contenedor-nombre whoami

# Ver capabilities del contenedor
docker exec contenedor-nombre capsh --print

# Ejecutar sin privilegios root
docker run --user nobody -d mi-app
```

### Limitación de recursos
```bash
# Limitar memoria
docker run --memory="512m" -d mi-app

# Limitar CPU
docker run --cpus="1.5" -d mi-app

# Limitar CPU por shares
docker run --cpu-shares=512 -d mi-app

# Limitar operaciones de I/O
docker run --device-read-bps /dev/sda:1mb -d mi-app

# Múltiples límites
docker run --memory="1g" --cpus="2" --ulimit nproc=1024:2048 -d mi-app
```

### Escáner de vulnerabilidades
```bash
# Escanear imagen (Docker Desktop)
docker scan mi-app:latest

# Ver vulnerabilidades detalladas
docker scan --severity high mi-app:latest

# Usar Trivy (herramienta externa)
trivy image mi-app:latest

# Usar Clair (herramienta externa)
clair-scanner mi-app:latest
```

### Secretos y variables sensibles
```bash
# Usar Docker secrets (Swarm mode)
echo "mi-password" | docker secret create db-password -

# Montar secret en contenedor
docker service create --secret db-password mi-app

# Usar archivos .env
docker run --env-file .env -d mi-app

# Variables de entorno desde archivo
docker run -e "$(cat .env | xargs)" -d mi-app
```

---

## ⚡ Comandos Avanzados

### Docker BuildKit
```bash
# Habilitar BuildKit
export DOCKER_BUILDKIT=1

# Build con BuildKit features
docker build --progress=plain -t mi-app .

# Build con cache mount
docker build --mount=type=cache,target=/app/node_modules -t mi-app .

# Build con secret mount
docker build --secret id=mypasswd,src=./password.txt -t mi-app .

# Build multi-plataforma
docker buildx build --platform linux/amd64,linux/arm64 -t mi-app .

# Crear builder personalizado
docker buildx create --name mybuilder
docker buildx use mybuilder
```

### Docker Contexts
```bash
# Listar contexts
docker context ls

# Crear context remoto
docker context create remote --docker "host=ssh://user@remote-host"

# Usar context específico
docker context use remote

# Ejecutar comando en context específico
docker --context remote ps

# Eliminar context
docker context rm remote
```

### Docker Swarm (básico)
```bash
# Inicializar swarm
docker swarm init

# Ver token para workers
docker swarm join-token worker

# Ver token para managers
docker swarm join-token manager

# Listar nodos
docker node ls

# Crear servicio
docker service create --name web --publish 80:80 nginx

# Listar servicios
docker service ls

# Escalar servicio
docker service scale web=3

# Eliminar servicio
docker service rm web

# Abandonar swarm
docker swarm leave --force
```

### Docker Machine (si está instalado)
```bash
# Crear máquina virtual
docker-machine create --driver virtualbox mi-maquina

# Listar máquinas
docker-machine ls

# Ver variables de entorno
docker-machine env mi-maquina

# Conectar a máquina
eval $(docker-machine env mi-maquina)

# SSH a máquina
docker-machine ssh mi-maquina

# Detener máquina
docker-machine stop mi-maquina

# Eliminar máquina
docker-machine rm mi-maquina
```

---

## 🔧 Troubleshooting

### Diagnóstico de problemas
```bash
# Ver información detallada del contenedor
docker inspect contenedor-nombre

# Ver logs con timestamps
docker logs -t contenedor-nombre

# Ver procesos dentro del contenedor
docker exec contenedor-nombre ps aux

# Ver uso de recursos
docker stats contenedor-nombre

# Ver variables de entorno
docker exec contenedor-nombre env

# Ver puertos abiertos
docker exec contenedor-nombre netstat -tuln

# Test de conectividad de red
docker exec contenedor-nombre ping google.com

# Ver archivos del sistema del contenedor
docker exec contenedor-nombre df -h
```

### Problemas comunes y soluciones
```bash
# Problema: "No space left on device"
docker system prune -a
docker volume prune

# Problema: Puerto ya en uso
docker ps --filter "publish=8080"
netstat -tulpn | grep 8080

# Problema: Contenedor no inicia
docker logs contenedor-nombre
docker run --rm -it imagen-nombre sh

# Problema: Permisos de archivos
docker exec -u root contenedor-nombre chown -R user:group /path

# Problema: DNS no funciona
docker run --dns 8.8.8.8 -it ubuntu nslookup google.com

# Problema: Imagen corrupta
docker rmi imagen:tag
docker pull imagen:tag

# Verificar integridad del daemon
docker system info
sudo systemctl status docker
```

### Debug de contenedores
```bash
# Ejecutar shell en contenedor problemático
docker exec -it contenedor-nombre /bin/bash

# O si bash no está disponible
docker exec -it contenedor-nombre /bin/sh

# Ejecutar como root para debug
docker exec -u 0 -it contenedor-nombre /bin/bash

# Ver archivos de configuración
docker exec contenedor-nombre cat /etc/nginx/nginx.conf

# Verificar procesos
docker exec contenedor-nombre ps aux

# Test de conectividad interna
docker exec contenedor-nombre ping otro-contenedor

# Verificar variables de entorno
docker exec contenedor-nombre printenv

# Debug de red
docker exec contenedor-nombre ip addr show
docker exec contenedor-nombre route -n

# Verificar montajes de volúmenes
docker exec contenedor-nombre mount | grep bind
```

### Recuperación de datos
```bash
# Copiar datos antes de eliminar contenedor
docker cp contenedor-nombre:/app/data ./backup/

# Crear imagen desde contenedor con problemas
docker commit contenedor-nombre backup-image:latest

# Exportar contenedor completo
docker export contenedor-nombre > contenedor-backup.tar

# Importar contenedor exportado
docker import contenedor-backup.tar nueva-imagen:latest

# Crear volumen de backup desde contenedor
docker run --volumes-from contenedor-nombre -v $(pwd):/backup ubuntu tar czf /backup/backup.tar.gz /data
```

---

## 📚 Ejemplos Prácticos por Tecnología

### Aplicación Node.js
```bash
# Dockerfile para Node.js
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
USER node
CMD ["npm", "start"]

# Comandos para desarrollo
docker build -t mi-node-app .
docker run -p 3000:3000 -v $(pwd):/app -d mi-node-app
docker logs -f mi-node-app
```

### Aplicación Python/Django
```bash
# Dockerfile para Python
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

# Con Docker Compose
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/app
    environment:
      - DEBUG=1
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Aplicación Java/Spring Boot
```bash
# Dockerfile para Java
FROM openjdk:11-jre-slim
COPY target/app.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]

# Multi-stage para Maven
FROM maven:3.8-openjdk-11 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package -DskipTests

FROM openjdk:11-jre-slim
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```

### Base de datos MySQL
```bash
# MySQL con persistencia
docker run --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=myapp \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=password \
  -v mysql_data:/var/lib/mysql \
  -p 3306:3306 \
  -d mysql:8

# Backup de MySQL
docker exec mysql-db mysqldump -u root -prootpassword myapp > backup.sql

# Restore de MySQL
docker exec -i mysql-db mysql -u root -prootpassword myapp < backup.sql
```

### Nginx como Proxy Reverso
```bash
# nginx.conf para proxy reverso
upstream app {
    server app1:3000;
    server app2:3000;
}

server {
    listen 80;
    location / {
        proxy_pass http://app;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

# Docker Compose con Nginx
version: '3.8'
services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app1
      - app2

  app1:
    build: .
    expose:
      - "3000"

  app2:
    build: .
    expose:
      - "3000"
```

---

## 🎯 Mejores Prácticas

### Optimización de imágenes
```bash
# 1. Usar imágenes base pequeñas
FROM alpine:latest
FROM node:16-alpine
FROM python:3.9-slim

# 2. Multi-stage builds
FROM node:16 AS builder
# build steps
FROM node:16-alpine
COPY --from=builder /app/dist ./dist

# 3. Minimizar layers
RUN apt-get update && apt-get install -y \
    package1 \
    package2 \
    && rm -rf /var/lib/apt/lists/*

# 4. Usar .dockerignore
node_modules
.git
*.log
```

### Seguridad
```dockerfile
# 1. Usuario no-root
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001
USER nextjs

# 2. Imagen base confiable
FROM node:16-alpine

# 3. Copiar solo lo necesario
COPY package*.json ./
RUN npm ci --only=production
COPY src ./src

# 4. Health checks
HEALTHCHECK --interval=30s --timeout=10s \
  CMD curl -f http://localhost:3000/health || exit 1
```

### Performance
```bash
# 1. Cache de layers
COPY package*.json ./
RUN npm install
COPY . .

# 2. Volúmenes para datos
docker run -v postgres_data:/var/lib/postgresql/data postgres

# 3. Límites de recursos
docker run --memory="512m" --cpus="1" mi-app

# 4. Usar BuildKit
export DOCKER_BUILDKIT=1
```

---

## 📋 Checklists

### Pre-deployment checklist
- [ ] Imagen optimizada (tamaño mínimo)
- [ ] Usuario no-root configurado
- [ ] Health check implementado
- [ ] Variables de entorno externalizadas
- [ ] Volúmenes para datos persistentes
- [ ] Límites de recursos configurados
- [ ] Logs estructurados
- [ ] Secrets manejados correctamente
- [ ] Red configurada apropiadamente
- [ ] Backup strategy definida

### Security checklist
- [ ] Imagen base actualizada
- [ ] Vulnerabilidades escaneadas
- [ ] Usuario no-root
- [ ] Capabilities mínimas
- [ ] Secrets no en variables de entorno
- [ ] Red segmentada
- [ ] Puertos mínimos expuestos
- [ ] File permissions correctos
- [ ] No hardcoded passwords
- [ ] TLS/SSL configurado

---

## 🔗 Comandos de Una Línea Útiles

```bash
# Eliminar todos los contenedores
docker rm -f $(docker ps -aq)

# Eliminar todas las imágenes
docker rmi -f $(docker images -q)

# Logs de todos los contenedores
docker ps -q | xargs -I {} docker logs {}

# IP de todos los contenedores
docker ps -q | xargs -I {} docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress}}' {}

# Eliminar imágenes por patrón
docker images | grep "pattern" | awk '{print $3}' | xargs docker rmi

# Backup de volumen rápido
docker run --rm -v vol_name:/data -v $(pwd):/backup alpine tar czf /backup/vol_backup.tar.gz -C /data .

# Estadísticas formateadas
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}"

# Encontrar contenedores que usan imagen específica
docker ps -a --filter ancestor=nginx

# Ver puertos de contenedores corriendo
docker ps --format "table {{.Names}}\t{{.Ports}}"

# Limpiar todo de golpe
docker system prune -a --volumes -f

# Crear red y conectar contenedores en una línea
docker network create mynet && docker run --network mynet --name app1 -d nginx && docker run --network mynet --name app2 -d nginx
```

---

## 📖 Recursos Adicionales

### Herramientas útiles complementarias
```bash
# Docker Compose V2
docker compose up  # (nuevo comando sin guión)

# Lazydocker (TUI para Docker)
lazydocker

# Dive (análisis de imágenes)
dive nginx:latest

# Trivy (scanner de vulnerabilidades)
trivy image nginx:latest

# Hadolint (linter para Dockerfiles)
hadolint Dockerfile

# Docker Scout (análisis de imágenes)
docker scout cves nginx:latest
```

### Archivos de configuración útiles
```bash
# ~/.docker/config.json (configuración de Docker)
{
  "auths": {},
  "credsStore": "desktop",
  "experimental": "enabled",
  "buildkit": true
}

# /etc/docker/daemon.json (configuración del daemon)
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "storage-driver": "overlay2"
}
```

Esta guía completa cubre todos los aspectos esenciales de Docker para el desarrollo día a día. Desde comandos básicos hasta configuraciones avanzadas, optimización, seguridad y troubleshooting. ¡Úsala como referencia rápida en tu trabajo diario!
