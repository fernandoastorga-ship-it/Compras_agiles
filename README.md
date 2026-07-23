# Compras Ágiles — despliegue gratuito

Arquitectura recomendada:

- **Render Free**: ejecuta la aplicación FastAPI.
- **Neon Free**: almacena permanentemente las compras y los adjuntos en PostgreSQL.
- **Costo mensual inicial: $0**.

> Importante: no uses una base PostgreSQL gratuita creada dentro de Render, porque esa base vence después de 30 días. La aplicación debe conectarse a Neon mediante `DATABASE_URL`.

## 1. Crear la base gratuita en Neon

1. Crea una cuenta en Neon.
2. Crea un proyecto nuevo.
3. Abre **Connect**.
4. Selecciona la conexión con pooling, cuando esté disponible.
5. Copia la cadena de conexión PostgreSQL completa. Se parece a:

```text
postgresql://usuario:contraseña@ep-algo-pooler.region.aws.neon.tech/neondb?sslmode=require
```

Guárdala. Se usará como `DATABASE_URL` en Render.

## 2. Subir el proyecto a GitHub

Sube el contenido de esta carpeta a un repositorio. `main.py`, `render.yaml` y `requirements.txt` deben quedar en la raíz.

Estructura esperada:

```text
main.py
render.yaml
requirements.txt
README.md
static/
  index.html
  app.js
  styles.css
```

No subas archivos `.db`, copias de seguridad ni contraseñas.

## 3. Publicar gratuitamente en Render

1. En Render selecciona **New > Blueprint**.
2. Conecta el repositorio de GitHub.
3. Render detectará `render.yaml`.
4. Completa las variables solicitadas:
   - `DATABASE_URL`: pega la conexión de Neon.
   - `APP_PASSWORD`: define la contraseña para entrar a la app.
5. Confirma el despliegue.

`SECRET_KEY` se genera automáticamente.

## 4. Primer ingreso

Cuando Render termine, abre la dirección `https://...onrender.com` e ingresa con `APP_PASSWORD`.

La primera ejecución crea automáticamente las tablas y los perfiles Fernando y Patricio.

## 5. Recuperar información antigua

La aplicación permite:

- Importar un backup JSON.
- Transferir datos antiguos que aún estén en `localStorage`, siempre que abras la nueva versión desde el mismo navegador y origen web donde estaban guardados.
- Exportar un backup JSON completo con compras y adjuntos.

## Límites de la alternativa gratuita

- Render suspende el servidor cuando pasa un periodo sin uso. La primera apertura posterior puede demorar alrededor de un minuto.
- Neon Free permite 0,5 GB por proyecto. Los datos de texto ocupan muy poco; los adjuntos consumen casi todo el espacio.
- La aplicación admite archivos de hasta 10 MB y un máximo de 30 MB de adjuntos por compra. Conviene utilizar PDF comprimidos y exportar backups periódicamente.

## Recomendación de respaldo

Una vez por semana o después de ingresar compras importantes:

1. Entra al perfil correspondiente.
2. Presiona **Exportar backup JSON**.
3. Guarda el archivo en Google Drive o OneDrive.

El backup incluye los documentos adjuntos.

## Ejecución local

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
$env:APP_PASSWORD="tu-clave"
uvicorn main:app --reload
```

Sin `DATABASE_URL`, la aplicación usa SQLite local solamente para desarrollo.
