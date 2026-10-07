# test-simple-stock-flow-infra

> **Prueba técnica · Ficha ADSO 3413974**  
> Infraestructura de contenedores y orquestación con Docker Compose.

---

### 1. Qué es esto
Es el orquestador de contenedores, redes y volúmenes persistentes de todo el sistema *Simple Stock Flow*. Provee el motor de base de datos MySQL 8.4 limpio (sin DDL previo, ya que las migraciones pertenecen a la API según ADR-001), el contenedor de la API en Laravel y el contenedor web Nginx con la aplicación React compilada. **No contiene** ninguna lógica de negocio ni código de aplicación.

### 2. Cómo se levanta
Desde un equipo limpio con Docker instalado:
```bash
# 1. Crear el archivo de entorno a partir del ejemplo
cp .env.example .env

# 2. Levantar el ecosistema completo
docker compose up --build -d
```
Los servicios estarán disponibles en:
* **Frontend Web (React):** `http://localhost:8080`
* **Backend API (Laravel):** `http://localhost:8000`
* **Base de datos (MySQL):** `localhost:3307` (mapeado internamente al puerto `3306`)

### 3. Dónde están los datos
* **Motor:** MySQL 8.4
* **Base de datos:** `stockflow`
* **Usuario:** `stockflow_user`
* **Puerto host:** `3307` (puerto de red interna en Docker: `3306`)
* **Credenciales:** Definidas en el archivo `.env` (`DB_PASSWORD`).
Para acceder con un cliente de terminal o DBeaver/Workbench:
```bash
mysql -h 127.0.0.1 -P 3307 -u stockflow_user -p stockflow
```

### 4. Cómo se prueba
Para verificar el estado de salud de todos los contenedores:
```bash
docker compose ps
```
Para ejecutar las pruebas y verificación arquitectónica dentro de la API:
```bash
docker compose run --rm api bash verify.sh
```

### 5. Qué falta
Toda la configuración declarativa de contenedores, los volúmenes con nombre para evitar problemas de permisos en Windows y la red puente `stockflow-network` están completamente configurados y operativos.