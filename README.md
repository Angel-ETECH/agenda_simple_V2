# Agenda Simple V2

Proyecto educativo con Django y MariaDB para registrar clientes y gestionar citas. Incluye inicio de sesión, búsqueda, edición y eliminación, con protección de clientes que tienen citas relacionadas.

## Instalación y ejecución

Comandos para PowerShell en la terminal de Visual Studio Code.

### 1. Descargar

```powershell
git clone https://github.com/Angel-ETECH/agenda_simple_V2.git
cd agenda_simple_V2
```

También se puede descargar desde **Code > Download ZIP** y abrir la carpeta que contiene `manage.py`.

### 2. Preparar Python

Entorno verificado: Python 3.14.9 de 64 bits, Django 6.1.3 y MariaDB 11.4.13. Este Django requiere MariaDB 10.11 o superior; MariaDB 10.4 incluido en algunas versiones de XAMPP no es compatible.

```powershell
py -3.14 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

No hace falta activar el entorno. Si `py` no está disponible, utilizar `python -m venv .venv` con la versión de Python indicada. No copiar `.venv` desde otra computadora.

### 3. Configurar MariaDB

Encender MariaDB. En `config/settings.py`, ajustar `DATABASES` al usuario, contraseña, host y puerto de la computadora donde se revisa. La configuración local usa `root`, contraseña vacía, `127.0.0.1` y puerto `3307`. Si el servidor del profesor escucha en `3306`, cambiar ese puerto.

En la consola SQL del servidor compatible, ejecutar:

```sql
CREATE DATABASE IF NOT EXISTS agenda_simple_v2_db
CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Esta base es independiente de la primera versión de Agenda Simple.

### 4. Crear las tablas y el usuario

```powershell
.\.venv\Scripts\python.exe manage.py migrate
.\.venv\Scripts\python.exe manage.py createsuperuser
.\.venv\Scripts\python.exe manage.py check
```

Elegir usuario, correo y contraseña. Al escribir la contraseña no se muestran caracteres: es normal. Las cuentas locales del alumno no están incluidas en el repositorio.

### 5. Ejecutar

```powershell
.\.venv\Scripts\python.exe manage.py runserver 8001
```

Abrir **http://127.0.0.1:8001/** e ingresar con la cuenta creada. El administrador de Django está en **http://127.0.0.1:8001/admin/**.

Mantener abierta la terminal. Para detener el servidor, presionar **Ctrl + C**. Si el puerto está ocupado, usar `runserver 8004` y abrir http://127.0.0.1:8004/.

Para ejecutarlo otro día solo se necesita encender MariaDB y repetir el comando `runserver`. No hace falta Apache ni ubicar el proyecto dentro de XAMPP.

## Recorrido para revisar

1. Iniciar sesión.
2. Registrar un cliente con nombre, apellido, correo, DNI y teléfono opcional.
3. Buscar y editar el cliente.
4. Crear una cita con asunto, fecha, hora, dirección y estado.
5. Editar y buscar citas.
6. Intentar eliminar el cliente con citas para comprobar la protección.
7. Eliminar la cita y luego el cliente de prueba.

## Archivos principales

- `apps/models/`: modelos Cliente y Cita, en archivos separados.
- `apps/views.py`: operaciones y validaciones.
- `apps/urls.py`: rutas de los módulos.
- `apps/templates/control/`: páginas HTML.
- `apps/static/control/`: estilos y JavaScript.
- `apps/migrations/`: migración inicial de V2.
- [GUIA_DOCENTE.md](GUIA_DOCENTE.md): explicación de los cambios y práctica guiada.

## Problemas comunes

- **Access denied:** revisar usuario y contraseña de MariaDB.
- **No se puede conectar:** encender MariaDB y revisar el puerto.
- **Unknown database:** crear `agenda_simple_v2_db`.
- **No module named django:** instalar las dependencias y usar el Python de `.venv`.
- **No puede entrar al admin:** utilizar la cuenta creada con `createsuperuser`.

Configuración destinada a revisión local. `DEBUG=True` y la clave de desarrollo no deben utilizarse en producción. Para configurar otra clave se admite la variable de entorno `DJANGO_SECRET_KEY`. El entorno virtual, los respaldos, los registros y los archivos privados no se publican.
