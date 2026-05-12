# Phase 1: n8n self-hosted + PostgreSQL + Caddy

## Guía de Uso
1. **Inicio de servicios:** `docker compose up -d` en la carpeta `infra/`.
2. **Apagado:** `docker compose down`.
3. **Acceso n8n:** Visita [http://localhost:5678](http://localhost:5678).
4. **Credenciales:** Usuario `admin`, contraseña configurada en `infra/.env`.
5. **HTTPS Local:** Caddy expone el servicio en `n8n.local` (requiere agregar Caddy root CA al host o ignorar advertencia de seguridad).
6. **Logs:** Para ver logs de n8n, usa `docker compose logs -f n8n`.
7. **Base de Datos:** Postgres persiste la data en el volumen de Docker.
