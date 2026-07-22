# Compras Ágiles — versión nube

Esta versión reemplaza `localStorage` por una API FastAPI y una base PostgreSQL. Las compras y sus adjuntos quedan almacenados fuera del navegador.

## Desarrollo local

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
pip install -r requirements.txt
set APP_PASSWORD=una-clave-segura
uvicorn main:app --reload
```

Sin `DATABASE_URL`, la app usa SQLite únicamente para pruebas locales.

## Despliegue recomendado en Render

1. Sube esta carpeta a un repositorio de GitHub.
2. En Render selecciona **New > Blueprint** y conecta el repositorio.
3. Render leerá `render.yaml` y creará:
   - un Web Service;
   - una base PostgreSQL `basic-1gb` persistente.
4. Cuando Render solicite `APP_PASSWORD`, define una contraseña segura.
5. Finalizado el despliegue, abre la URL `onrender.com` e inicia sesión.

## Migración de información antigua

- Si se publica sobre la misma URL/origen del navegador antiguo, la pantalla inicial detectará `localStorage` y mostrará **Transferir a la nube**.
- Si antes exportaste un backup JSON, entra a un perfil y usa **Importar backup**.
- Si el navegador ya eliminó el `localStorage` y no existe backup, la aplicación no puede reconstruir esos datos.

## Variables de entorno

- `DATABASE_URL`: conexión PostgreSQL. Render la completa desde la base.
- `APP_PASSWORD`: contraseña común de acceso.
- `SECRET_KEY`: firma de la sesión; Render la genera.
- `MAX_FILE_BYTES`: límite por archivo, por defecto 10 MB.
- `MAX_PURCHASE_FILES_BYTES`: límite total de adjuntos por compra, por defecto 30 MB.

## Respaldo

La aplicación mantiene la exportación JSON. Conviene descargar un backup periódicamente aunque PostgreSQL sea persistente.
