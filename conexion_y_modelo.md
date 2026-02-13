# Actividad M7 L2 – Conexión a Base de Datos y Uso del ORM en Django

## 1. Conexión a PostgreSQL

Para conectar Django con PostgreSQL se instaló el driver necesario utilizando el siguiente comando:

```bash
pip install psycopg2-binary
```

Posteriormente, se creó una base de datos vacía llamada **libreria** en PostgreSQL.

En el archivo `settings.py` del proyecto Django se configuró la conexión a la base de datos de la siguiente manera:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'libreria',
        'USER': 'usuario',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

> Por motivos de seguridad, las credenciales reales no se incluyen en este archivo.

---

## 2. Definición del Modelo

En la aplicación principal se definió el modelo **Libro** en el archivo `models.py`:

```python
from django.db import models

class Libro(models.Model):
    titulo = models.CharField(max_length=200)
    autor = models.CharField(max_length=100)
    fecha_publicacion = models.DateField()
    disponible = models.BooleanField(default=True)

    def __str__(self):
        return self.titulo
```

### Clave primaria

Django crea automáticamente un campo `id` como clave primaria si no se define explícitamente.

### Clave primaria compuesta

Django no soporta claves primarias compuestas de forma nativa. Sin embargo, se puede simular una restricción de unicidad usando:

```python
class Meta:
    unique_together = ('titulo', 'autor')
```

---

## 3. Aplicación de Migraciones

Se ejecutaron los siguientes comandos:

```bash
python manage.py makemigrations
```

Este comando detecta los cambios en los modelos y genera los archivos de migración.

```bash
python manage.py migrate
```

Este comando aplica las migraciones y crea las tablas correspondientes en la base de datos PostgreSQL.

---

## 4. Operaciones CRUD usando el ORM (Shell de Django)

Se utilizó el shell de Django para realizar las operaciones CRUD.

### Ingreso al shell

```bash
python manage.py shell
```

```python
from principal.models import Libro
```

### Crear un libro

```python
Libro.objects.create(
    titulo="El principito",
    autor="Antoine de Saint-Exupéry",
    fecha_publicacion="1943-04-06",
    disponible=True
)
```

### Listar todos los libros

```python
Libro.objects.all()
```

### Buscar un libro por su título

```python
Libro.objects.get(titulo="El principito")
```

O alternativamente:

```python
Libro.objects.filter(titulo="El principito")
```

### Actualizar el campo disponible

```python
libro = Libro.objects.get(titulo="El principito")
libro.disponible = False
libro.save()
```

### Eliminar un libro

```python
libro = Libro.objects.get(titulo="1984")
libro.delete()
```

---

## 5. Verificación en PostgreSQL

Los registros creados desde el shell de Django se verificaron directamente en PostgreSQL utilizando la herramienta pgAdmin 4 y consultas SQL como:

```sql
SELECT * FROM principal_libro;
```

Esto confirmó que la conexión entre Django y PostgreSQL funciona correctamente y que los datos se almacenan de forma persistente.

---

## Conclusión

La actividad permitió comprender la conexión entre Django y PostgreSQL, la definición de modelos, la aplicación de migraciones y el uso del ORM para realizar operaciones CRUD de manera eficiente.
