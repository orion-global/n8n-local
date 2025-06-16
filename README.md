# n8n-orion - Automatización de Flujos de Trabajo

Este proyecto contiene una configuración de n8n con Docker Compose que incluye PostgreSQL y pgvector para automatización avanzada de flujos de trabajo.

## 📋 Requisitos Previos

- Docker
- Docker Compose

## 🐳 Instalación de Docker

### Windows

#### Opción 1: Docker Desktop (Recomendado)
1. Descarga Docker Desktop desde [docker.com](https://www.docker.com/products/docker-desktop/)
2. Ejecuta el instalador descargado
3. Sigue las instrucciones del asistente de instalación
4. Reinicia tu computadora cuando se te solicite
5. Abre Docker Desktop y completa la configuración inicial

#### Opción 2: Usando Chocolatey
```powershell
# Instala Chocolatey si no lo tienes
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Instala Docker Desktop
choco install docker-desktop
```

#### Opción 3: Usando Winget
```powershell
winget install Docker.DockerDesktop
```

### macOS

#### Opción 1: Docker Desktop (Recomendado)
1. Descarga Docker Desktop desde [docker.com](https://www.docker.com/products/docker-desktop/)
2. Abre el archivo `.dmg` descargado
3. Arrastra Docker a la carpeta Aplicaciones
4. Abre Docker desde Aplicaciones
5. Sigue las instrucciones de configuración inicial

#### Opción 2: Usando Homebrew
```bash
# Instala Homebrew si no lo tienes
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instala Docker Desktop
brew install --cask docker
```

### Linux

#### Ubuntu/Debian
```bash
# Actualiza el sistema
sudo apt update

# Instala dependencias
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# Agrega la clave GPG oficial de Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Agrega el repositorio de Docker
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Actualiza los paquetes
sudo apt update

# Instala Docker
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Inicia Docker
sudo systemctl start docker
sudo systemctl enable docker

# Agrega tu usuario al grupo docker (opcional, para evitar usar sudo)
sudo usermod -aG docker $USER
```

#### CentOS/RHEL/Fedora
```bash
# Para CentOS/RHEL
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Para Fedora
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Inicia Docker
sudo systemctl start docker
sudo systemctl enable docker

# Agrega tu usuario al grupo docker (opcional)
sudo usermod -aG docker $USER
```

#### Arch Linux
```bash
sudo pacman -S docker docker-compose
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

## 🚀 Configuración del Proyecto

### 1. Clona o descarga el proyecto
```bash
git clone https://github.com/orion-global/n8n-local.git
cd n8n-local
```

### 2. Verifica la instalación de Docker
```bash
docker --version
docker-compose --version
```

### 3. Ejecuta el proyecto
```bash
# Inicia todos los servicios
docker-compose up -d

# Para ver los logs en tiempo real
docker-compose logs -f

# Para ver el estado de los contenedores
docker-compose ps
```

## 🔧 Servicios Incluidos

- **n8n**: Plataforma de automatización de flujos de trabajo
- **PostgreSQL**: Base de datos principal
- **pgvector**: Extensión de PostgreSQL para búsquedas vectoriales

## 🌐 Acceso a la Aplicación

Una vez que los contenedores estén ejecutándose:

- **n8n Interface**: http://localhost:5678
- **Credenciales por defecto**:
  - Usuario: `admin`
  - Contraseña: `admin_password`

## 📊 Comandos Útiles

### Gestión de Contenedores
```bash
# Detener todos los servicios
docker-compose down

# Detener y eliminar volúmenes (¡CUIDADO! Esto borrará todos los datos)
docker-compose down -v

# Reiniciar un servicio específico
docker-compose restart n8n

# Ver logs de un servicio específico
docker-compose logs -f n8n

# Acceder al shell de un contenedor
docker-compose exec n8n bash
docker-compose exec postgres psql -U n8n -d n8n
```

### Mantenimiento
```bash
# Actualizar las imágenes
docker-compose pull

# Reconstruir y reiniciar
docker-compose up -d --build

# Limpiar imágenes no utilizadas
docker image prune -f

# Ver uso de espacio
docker system df
```

## 🔒 Configuración de Seguridad

### Cambiar Credenciales por Defecto
Edita el archivo `docker-compose.yml` y modifica:

```yaml
environment:
  - N8N_BASIC_AUTH_USER=tu_usuario
  - N8N_BASIC_AUTH_PASSWORD=tu_contraseña_segura
  - POSTGRES_PASSWORD=tu_contraseña_db_segura
```

### Variables de Entorno Recomendadas
Considera crear un archivo `.env` para variables sensibles:

```bash
# .env
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=contraseña_muy_segura
POSTGRES_PASSWORD=contraseña_db_muy_segura
```

## 🛠️ Solución de Problemas

### Puerto ya en uso
```bash
# Verificar qué proceso usa el puerto 5678
sudo lsof -i :5678

# Cambiar el puerto en docker-compose.yml
ports:
  - "8080:5678"  # Cambia 5678 por otro puerto disponible
```

### Problemas de permisos (Linux)
```bash
# Reinicia la sesión después de agregar el usuario al grupo docker
newgrp docker

# O reinicia el sistema
sudo reboot
```

### Contenedores no inician
```bash
# Verificar logs detallados
docker-compose logs

# Verificar estado del sistema Docker
sudo systemctl status docker

# Limpiar y reiniciar
docker-compose down
docker system prune -f
docker-compose up -d
```

## 📚 Recursos Adicionales

- [Documentación oficial de n8n](https://docs.n8n.io/)
- [Documentación de Docker](https://docs.docker.com/)
- [Documentación de Docker Compose](https://docs.docker.com/compose/)

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Haz fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la licencia [especifica tu licencia aquí].

---

¿Necesitas ayuda? Abre un issue en el repositorio del proyecto.
