# Листинг скриптов запуска

Скрипты запускают PostgreSQL и бэкенд через Docker Compose. Выполнять из корня репозитория (скрипты сами переходят в корень).

---

## Windows: scripts/start.bat

```batch
@echo off
REM Start Dashboard Battle: PostgreSQL + backend
REM Requires Docker and Docker Compose (docker compose or docker-compose)

set COMPOSE_CMD=
where docker >nul 2>&1
if %ERRORLEVEL% neq 0 (
    echo Docker not found in PATH. Install Docker Desktop or Docker Engine.
    exit /b 1
)

docker compose version >nul 2>&1
if %ERRORLEVEL% equ 0 (
    set COMPOSE_CMD=docker compose
) else (
    docker-compose version >nul 2>&1
    if %ERRORLEVEL% equ 0 (
        set COMPOSE_CMD=docker-compose
    ) else (
        echo Docker Compose not found. Install Docker Desktop or docker-compose.
        exit /b 1
    )
)

cd /d "%~dp0.."
echo Starting PostgreSQL and backend...
%COMPOSE_CMD% up -d
if %ERRORLEVEL% neq 0 (
    echo Failed to start. Check Docker is running.
    exit /b 1
)
echo Started. Backend: http://localhost:8080  Postgres: localhost:5432
```

**Использование:** из командной строки Windows выполнить `scripts\start.bat` (из любого каталога) или дважды щёлкнуть по файлу. Требуется установленный Docker и Docker Compose (или Docker Desktop).

---

## Linux / macOS: scripts/start.sh

```sh
#!/bin/sh
# Start Dashboard Battle: PostgreSQL + backend
# Requires Docker and Docker Compose (docker compose or docker-compose)

set -e
cd "$(dirname "$0")/.."

if ! command -v docker >/dev/null 2>&1; then
    echo "Docker not found. Install Docker Engine or Docker Desktop."
    exit 1
fi

if docker compose version >/dev/null 2>&1; then
    COMPOSE_CMD="docker compose"
elif command -v docker-compose >/dev/null 2>&1; then
    COMPOSE_CMD="docker-compose"
else
    echo "Docker Compose not found. Install docker-compose or use Docker Desktop."
    exit 1
fi

echo "Starting PostgreSQL and backend..."
$COMPOSE_CMD up -d
echo "Started. Backend: http://localhost:8080  Postgres: localhost:5432"
```

**Использование:** сделать скрипт исполняемым (`chmod +x scripts/start.sh`) и выполнить `./scripts/start.sh` из корня репозитория или из любого каталога (скрипт перейдёт в корень).

---

## Пояснения

- Оба скрипта проверяют наличие `docker` и затем `docker compose` или `docker-compose`.
- Запуск: `docker compose up -d` (или `docker-compose up -d`) — контейнеры работают в фоне.
- Остановка: вручную выполнить в корне проекта `docker compose down` (или `docker-compose down`).
