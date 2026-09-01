# Despliegue de Entorno de Base de Datos e Infraestructura Docker (WSL2 / Ubuntu 24.04)

Este repositorio contiene la configuración, scripts de orquestación y documentación para el despliegue de múltiples motores de bases de datos relacionales en contenedores Docker sobre WSL2 (Ubuntu 24.04 LTS).

---

## 🛠️ Requisitos Previos e Instalación del Entorno

### 1. Inicialización de WSL2 y Distribución
Desde PowerShell como Administrador:
```powershell
wsl --install
wsl --install -d Ubuntu-24.04
```

### 2. Verificación de Contenedores y Estructura
```bash
# Validar versiones de Docker
docker --version
docker compose version

# Crear estructura de directorios del laboratorio
mkdir -p ~/ia-lab/services/motores-bd/{mysql,postgres,mssql,oracle} ~/ia-lab/data/{mysql,postgres,mssql,oracle}

# Crear red interna de Docker compartida
docker network create ia-lab-network
```

---

## 🚀 Motores de Base de Datos Desplegados

### 1. MySQL 8.0
- **Puerto Host:** `3306`
- **Volumen Datos:** `./data/mysql`
- **Servicio Compose:** `services/motores-bd/mysql`
- **Comando de despliegue:**
  ```bash
  cd ~/ia-lab/services/motores-bd/mysql && docker compose up -d
  ```

### 2. PostgreSQL 17
- **Puerto Host:** `5432` / `5433`
- **Volumen Datos:** `./data/postgres`
- **Servicio Compose:** `services/motores-bd/postgres`
- **Comando de despliegue:**
  ```bash
  cd ~/ia-lab/services/motores-bd/postgres && docker compose up -d
  ```

### 3. Microsoft SQL Server 2022
- **Puerto Host:** `1433`
- **Volumen Datos:** `./data/mssql`
- **Herramientas Cliente:** `mssql-tools18` (`sqlcmd`)
- **Servicio Compose:** `services/motores-bd/mssql`
- **Comando de despliegue:**
  ```bash
  cd ~/ia-lab/services/motores-bd/mssql && docker compose up -d
  ```

### 4. Oracle XE 21c
- **Puerto Host:** `1521`
- **Volumen Datos:** `./data/oracle` (Permisos asignados: `54321:54321`)
- **Service Name (PDB):** `tecnogua`
- **Servicio Compose:** `services/motores-bd/oracle`
- **Comando de despliegue:**
  ```bash
  cd ~/ia-lab/services/motores-bd/oracle && docker compose up -d
  ```

---

## 🔌 Conexión Externa y Administración (DBeaver)

1. Obtener la IP virtual de WSL2:
   ```bash
   ip a
   ```
2. Configurar el cliente gráfico (DBeaver, DataGrip, Navicat) utilizando la IP de WSL2, el puerto asignado por cada motor y las credenciales configuradas en los archivos `.env` respectivos.

---

## 📁 Estructura del Proyecto

```text
~/ia-lab/
├── services/
│   └── motores-bd/
│       ├── mysql/       # Docker Compose + .env + README
│       ├── postgres/    # Docker Compose + .env + README
│       ├── mssql/       # Docker Compose + .env + README
│       └── oracle/      # Docker Compose + .env + README
└── data/                # Volúmenes de datos persistentes
    ├── mysql/
    ├── postgres/
    ├── mssql/
    └── oracle/
```
