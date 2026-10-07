# test-simple-stock-flow-infra

> **Prueba tÃ©cnica Â· Ficha ADSO 3413974**  
> Infraestructura de contenedores y orquestaciÃ³n con Docker Compose.

---

### 1. QuÃ© es esto
Es el orquestador de contenedores, redes y volÃºmenes persistentes de todo el sistema *Simple Stock Flow*. Provee el motor de base de datos MySQL 8.4 limpio (sin DDL previo, ya que las migraciones pertenecen a la API segÃºn ADR-001), el contenedor de la API en Laravel y el contenedor web Nginx con la aplicaciÃ³n React compilada. **No contiene** ninguna lÃ³gica de negocio ni cÃ³digo de aplicaciÃ³n.

### 2. CÃ³mo se levanta
Desde un equipo limpio con Docker instalado:
```bash
# 1. Crear el archivo de entorno a partir del ejemplo
cp .env.example .env

# 2. Levantar el ecosistema completo
docker compose up --build -d
```
Los servicios estarÃ¡n disponibles en:
* **Frontend Web (React):** `http://localhost:8080`
* **Backend API (Laravel):** `http://localhost:8000`
* **Base de datos (MySQL):** `localhost:3306`

### 3. DÃ³nde estÃ¡n los datos
* **Motor:** MySQL 8.4
* **Base de datos:** `stockflow`
* **Usuario:** `stockflow_user`
* **Puerto host:** `3306` (puerto de red interna: `3306`)
* **Credenciales:** Definidas en el archivo `.env` (`DB_PASSWORD`).
Para acceder con un cliente de terminal o DBeaver/Workbench:
```bash
mysql -h 127.0.0.1 -P 3306 -u stockflow_user -p stockflow
```

### 4. CÃ³mo se prueba
Para verificar el estado de salud de todos los contenedores:
```bash
docker compose ps
```
Para ejecutar las pruebas y verificaciÃ³n arquitectÃ³nica dentro de la API:
```bash
docker compose run --rm api bash verify.sh
```

### 5. QuÃ© falta
Toda la configuraciÃ³n declarativa de contenedores, volÃºmenes con nombre para evitar problemas de permisos en Windows y la red puente `stockflow-network` estÃ¡n completas y operativas.