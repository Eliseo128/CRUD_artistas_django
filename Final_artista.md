¡Absolutamente! Mejorar la propuesta para incluir el modelo `Cancion` desde el principio es una idea excelente. Hace el proyecto mucho más completo y realista, permitiendo a los estudiantes entender conceptos clave como las relaciones entre modelos.

Aquí tienes la guía de la Clase 2, totalmente reestructurada y enriquecida para incluir el CRUD de Artistas y la gestión de sus Canciones. Cada paso está explicado con detalle para que sea fácil de seguir.

---

### **📘 Clase 2: Django con Python para Aplicaciones Web Profesionales**

**¡Bienvenidos!** En esta clase construiremos una aplicación web completa de música. No solo gestionaremos una lista de artistas, sino que también asociaremos canciones a cada uno de ellos, aprendiendo a manejar relaciones en la base de datos. Al final, tendrás un proyecto funcional con un diseño atractivo.

**📍 Nivel:** Principiante
**📍 Sistema Operativo:** Windows
**📍 Tecnologías:** Python, Django, HTML, CSS
**📍 Objetivos de Aprendizaje:**
*   Crear un proyecto Django con una aplicación personalizada.
*   Definir y relacionar dos modelos: `Artista` y `Cancion` (relación uno a muchos).
*   Configurar el panel de administración de Django para gestionar ambos modelos.
*   Implementar la funcionalidad **CRUD** (Crear, Leer, Actualizar, Borrar) completa para los artistas.
*   Mostrar las canciones asociadas a cada artista en su página de detalle.
*   Utilizar plantillas de Django, incluyendo herencia (`extends`) y archivos estáticos (CSS).
*   Diseñar formularios para la entrada de datos del usuario.

---

### **Paso 1: Preparación del Entorno y Creación del Proyecto**

**Explicación:** Vamos a crear la estructura de carpetas, configurar un entorno virtual para aislar nuestro proyecto y, finalmente, crear el proyecto y la aplicación de Django.

**Acciones en la Terminal:**

```bash
# 1. Abre la terminal (PowerShell o CMD) y navega a tu carpeta de desarrollo (ej. Documentos).

# 2. Crea la carpeta principal del curso y entra en ella.
mkdir Curso_Django
cd Curso_Django

# 3. Crea la carpeta para este proyecto específico y entra en ella.
mkdir Artista
cd Artista

# 4. Abre la carpeta del proyecto en Visual Studio Code.
code .

# 5. En la nueva terminal de VS Code (Ctrl+Ñ o Terminal > Nueva terminal):
#    Verifica que Python y pip estén instalados.
python --version
pip --version

# 6. Crea un entorno virtual. Esto crea una "caja" para las librerías de este proyecto.
python -m venv .venv

# 7. Activa el entorno virtual. Verás (.venv) al inicio de la línea de la terminal.
.\.venv\Scripts\activate

# 8. Instala Django y Pillow (Pillow es para manejar la subida de imágenes).
pip install django pillow

# 9. Crea el proyecto Django. El punto '.' evita crear una carpeta extra.
django-admin startproject backend_artista .

# 10. Crea la aplicación que manejará la lógica de los artistas y canciones.
python manage.py startapp app_artista
```

---

### **Paso 2: Configuración del Proyecto Principal**

**Explicación:** Ahora debemos decirle a Django que nuestra `app_artista` existe y configurar dónde se guardarán los archivos estáticos (CSS) y los archivos media (fotos subidas por el usuario).

**1. Registrar la aplicación en `backend_artista/settings.py`**

Busca la lista `INSTALLED_APPS` y añade `'app_artista'` al final.

```python
# backend_artista/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Nuestra aplicación debe estar registrada aquí.
    'app_artista',
]
```

**2. Configurar archivos estáticos y media en `backend_artista/settings.py`**

Añade estas líneas al final del archivo.

```python
# backend_artista/settings.py (al final del archivo)
import os

# Configuración para archivos estáticos (CSS, JavaScript)
STATIC_URL = '/static/'
# STATICFILES_DIRS le dice a Django dónde buscar archivos estáticos además de en las apps.
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'app_artista/static')]

# Configuración para archivos subidos por el usuario (fotos)
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

**3. Configurar las URLs principales en `backend_artista/urls.py`**

Le diremos al proyecto que cualquier URL que no sea `/admin/` debe ser gestionada por nuestra `app_artista`.

```python
# backend_artista/urls.py

from django.contrib import admin
from django.urls import path, include  # 'include' es clave para conectar apps
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    # Redirige todas las demás peticiones a las URLs de nuestra app.
    path('', include('app_artista.urls')),
]

# Esto es necesario para que el servidor de desarrollo pueda mostrar las imágenes subidas.
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

### **Paso 3: Definir los Modelos de Datos (`Artista` y `Cancion`)**

**Explicación:** Un modelo es la representación de una tabla en la base de datos. Crearemos un modelo para `Artista` y otro para `Cancion`. Usaremos una `ForeignKey` para establecer que **un artista puede tener muchas canciones**, pero una canción pertenece a un solo artista.

**Edita el archivo `app_artista/models.py`:**

```python
# app_artista/models.py

from django.db import models

# Modelo para los Artistas.
class Artista(models.Model):
    nombre = models.CharField(max_length=100, help_text="Nombre del artista o banda")
    nacionalidad = models.CharField(max_length=100)
    # ImageField requiere Pillow. upload_to crea una subcarpeta para las imágenes.
    foto_artista = models.ImageField(upload_to='img_artistas/', blank=True, null=True)

    def __str__(self):
        # Esto es lo que se mostrará en el panel de administración.
        return self.nombre

    class Meta:
        verbose_name = "Artista"
        verbose_name_plural = "Artistas"

# Modelo para las Canciones.
class Cancion(models.Model):
    titulo = models.CharField(max_length=100)
    # ForeignKey establece la relación. Un Artista tiene muchas Canciones.
    # on_delete=models.CASCADE: Si se borra un artista, se borran sus canciones.
    # related_name='canciones': Nos permite acceder a las canciones desde un artista
    # (ej: mi_artista.canciones.all()).
    artista = models.ForeignKey(Artista, on_delete=models.CASCADE, related_name='canciones')

    def __str__(self):
        return f"{self.titulo} - {self.artista.nombre}"

    class Meta:
        verbose_name = "Canción"
        verbose_name_plural = "Canciones"
```

---

### **Paso 4: Aplicar Cambios a la Base de Datos y Crear Superusuario**

**Explicación:** Cada vez que creamos o modificamos un modelo, debemos decirle a Django que prepare los cambios (`makemigrations`) y luego los aplique a la base de datos (`migrate`). Después, crearemos un superusuario para acceder al panel de administración.

**Acciones en la Terminal:**

```bash
# 1. Crea los archivos de migración basados en los cambios en models.py.
python manage.py makemigrations

# 2. Aplica esas migraciones a la base de datos (creará las tablas).
python manage.py migrate

# 3. Crea un superusuario para acceder al panel de administración.
python manage.py createsuperuser

# Sigue las instrucciones: introduce un nombre de usuario, email (opcional) y contraseña.
```

---

### **Paso 5: Configurar el Panel de Administración**

**Explicación:** Para poder gestionar Artistas y Canciones desde el panel de admin de Django, necesitamos registrarlos.

**Edita el archivo `app_artista/admin.py`:**

```python
# app_artista/admin.py

from django.contrib import admin
from .models import Artista, Cancion

# Registramos los modelos para que aparezcan en el panel de admin.
admin.site.register(Artista)
admin.site.register(Cancion)
```

**¡Prueba el panel de admin!**
1.  Inicia el servidor: `python manage.py runserver`
2.  Abre tu navegador y ve a `http://127.0.0.1:8000/admin/`
3.  Inicia sesión con el superusuario que creaste.
4.  ¡Ya puedes ver las secciones "Artistas" y "Canciones"! **Agrega 2 o 3 artistas con un par de canciones cada uno para tener datos de prueba.**

---

### **Paso 6: Crear el Formulario para Artistas**

**Explicación:** Usaremos los `ModelForm` de Django para crear formularios HTML de manera automática y segura a partir de nuestro modelo `Artista`.

**Crea un nuevo archivo `app_artista/forms.py`:**

```python
# app_artista/forms.py

from django import forms
from .models import Artista

# Django creará un formulario basado en el modelo Artista.
class ArtistaForm(forms.ModelForm):
    class Meta:
        model = Artista
        # Especificamos qué campos del modelo queremos en el formulario.
        fields = ['nombre', 'nacionalidad', 'foto_artista']
```

---

### **Paso 7: Desarrollar las Vistas (la Lógica de la Aplicación)**

**Explicación:** Las vistas son funciones que reciben una petición del usuario, procesan la lógica (consultar la base de datos, validar un formulario, etc.) y devuelven una respuesta, generalmente renderizando una plantilla HTML.

**Edita el archivo `app_artista/views.py`:**

```python
# app_artista/views.py

from django.shortcuts import render, get_object_or_404, redirect
from .models import Artista
from .forms import ArtistaForm

# Vista para la página principal: Lista de todos los artistas.
def listar_artistas(request):
    artistas = Artista.objects.all()  # Obtiene todos los objetos Artista.
    return render(request, 'listar_artistas.html', {'artistas': artistas})

# Vista para ver los detalles de un artista específico y sus canciones.
def detalle_artista(request, artista_id):
    # get_object_or_404 busca un objeto; si no lo encuentra, muestra un error 404.
    artista = get_object_or_404(Artista, id=artista_id)
    return render(request, 'detalle_artista.html', {'artista': artista})

# Vista para crear un nuevo artista.
def crear_artista(request):
    if request.method == 'POST':  # Si el formulario fue enviado...
        form = ArtistaForm(request.POST, request.FILES)  # request.FILES es para las imágenes.
        if form.is_valid():  # Si los datos son correctos...
            form.save()  # Guarda el nuevo artista.
            return redirect('listar_artistas')  # Redirige a la lista de artistas.
    else:  # Si es la primera vez que se carga la página...
        form = ArtistaForm()  # Muestra un formulario vacío.
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Crear Artista'})

# Vista para editar un artista existente.
def editar_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        # 'instance=artista' llena el formulario con los datos del artista existente.
        form = ArtistaForm(request.POST, request.FILES, instance=artista)
        if form.is_valid():
            form.save()
            return redirect('detalle_artista', artista_id=artista.id)
    else:
        form = ArtistaForm(instance=artista)
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Editar Artista'})

# Vista para borrar un artista.
def borrar_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':  # Si el usuario confirma el borrado...
        artista.delete()
        return redirect('listar_artistas')
    # Si es GET, muestra la página de confirmación.
    return render(request, 'confirmar_borrar.html', {'artista': artista})
```

---

### **Paso 8: Definir las Rutas de la Aplicación (URLs)**

**Explicación:** Las URLs conectan una dirección web con una vista específica. Crearemos las rutas para todas nuestras vistas.

**Crea el archivo `app_artista/urls.py`:**

```python
# app_artista/urls.py

from django.urls import path
from . import views

urlpatterns = [
    # URL principal: http://127.0.0.1:8000/
    path('', views.listar_artistas, name='listar_artistas'),
    # URL para ver el detalle de un artista: /artista/1/
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    # URL para crear un artista: /crear/
    path('crear/', views.crear_artista, name='crear_artista'),
    # URL para editar un artista: /editar/1/
    path('editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    # URL para la página de confirmación de borrado: /borrar/1/
    path('borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

---

### **Paso 9: Crear las Plantillas HTML**

**Explicación:** Las plantillas son los archivos HTML que verá el usuario. Usaremos el lenguaje de plantillas de Django para mostrar datos dinámicos y crearemos una plantilla `base.html` para no repetir código.

**Crea las siguientes carpetas y archivos:**
1.  Dentro de `app_artista/`, crea la carpeta `templates/`.
2.  Dentro de `app_artista/`, crea la carpeta `static/` y dentro de ella `css/`.

**1. `app_artista/templates/base.html` (La plantilla esqueleto)**

```html
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Melodías</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}">
</head>
<body>
    <div class="container">
        <header>
            <h1><a href="{% url 'listar_artistas' %}">Melodías</a></h1>
            <nav>
                <a href="{% url 'listar_artistas' %}">Artistas</a>
            </nav>
        </header>
        <main>
            {% block contenido %}
            <!-- El contenido de las otras plantillas se insertará aquí -->
            {% endblock %}
        </main>
    </div>
</body>
</html>
```

**2. `app_artista/templates/listar_artistas.html`**

```html
{% extends 'base.html' %}

{% block contenido %}
<div class="header-action">
    <h2>Artistas</h2>
    <a href="{% url 'crear_artista' %}" class="btn btn-primary">(+) Agregar Artista</a>
</div>
<ul class="item-list">
    {% for artista in artistas %}
    <li>
        <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
    </li>
    {% empty %}
    <li>No hay artistas registrados todavía.</li>
    {% endfor %}
</ul>
{% endblock %}
```

**3. `app_artista/templates/detalle_artista.html`**

```html
{% extends 'base.html' %}

{% block contenido %}
<div class="detail-header">
    <div class="detail-info">
        <h2>{{ artista.nombre }}</h2>
        <h4>{{ artista.nacionalidad }}</h4>
    </div>
    {% if artista.foto_artista %}
        <img src="{{ artista.foto_artista.url }}" alt="Foto de {{ artista.nombre }}" class="detail-img">
    {% endif %}
</div>

<div class="actions">
    <a href="{% url 'editar_artista' artista.id %}" class="btn btn-secondary">Editar</a>
    <a href="{% url 'borrar_artista' artista.id %}" class="btn btn-danger">Borrar</a>
</div>

<h3>Canciones</h3>
<ul class="item-list">
    {% for cancion in artista.canciones.all %}
    <li>{{ cancion.titulo }}</li>
    {% empty %}
    <li>Este artista no tiene canciones registradas.</li>
    {% endfor %}
</ul>
{% endblock %}
```

**4. `app_artista/templates/formulario_artista.html`**

```html
{% extends 'base.html' %}

{% block contenido %}
<h2>{{ titulo }}</h2>
<form method="post" enctype="multipart/form-data" class="form-styled">
    {% csrf_token %}
    {{ form.as_p }}
    <div class="actions">
        <button type="submit" class="btn btn-primary">Guardar</button>
        <a href="{% url 'listar_artistas' %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

**5. `app_artista/templates/confirmar_borrar.html`**

```html
{% extends 'base.html' %}

{% block contenido %}
<h2>¿Estás seguro de que quieres borrar a "{{ artista.nombre }}"?</h2>
<p>Esta acción no se puede deshacer.</p>
<form method="post">
    {% csrf_token %}
    <div class="actions">
        <button type="submit" class="btn btn-danger">Sí, Borrar</button>
        <a href="{% url 'detalle_artista' artista.id %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

---

### **Paso 10: Añadir Estilos CSS**

**Explicación:** Para que nuestra aplicación no solo sea funcional sino también atractiva, añadiremos estilos CSS.

**Crea el archivo `app_artista/static/css/styles.css`:**

```css
/* app_artista/static/css/styles.css */
body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    background-color: #f8f9fa;
    color: #343a40;
    margin: 0;
}

.container {
    max-width: 900px;
    margin: 20px auto;
    padding: 20px;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

header h1 a {
    color: #007bff;
    text-decoration: none;
}

nav a {
    color: #6c757d;
    text-decoration: none;
    margin-left: 15px;
    font-size: 1.1em;
}

.header-action { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #dee2e6; margin-bottom: 20px; }

.item-list { list-style: none; padding: 0; }
.item-list li {
    padding: 15px;
    background-color: #f1f3f5;
    border-radius: 5px;
    margin-bottom: 10px;
    font-size: 1.1em;
}
.item-list li a { text-decoration: none; color: #495057; font-weight: 500; }
.item-list li a:hover { color: #007bff; }

.detail-header { display: flex; gap: 30px; margin-bottom: 20px; }
.detail-img { width: 200px; height: 200px; object-fit: cover; border-radius: 8px; }

.actions { margin-top: 20px; margin-bottom: 20px; display: flex; gap: 10px; }

.btn {
    display: inline-block;
    padding: 10px 20px;
    font-size: 1em;
    font-weight: 600;
    text-align: center;
    text-decoration: none;
    border-radius: 5px;
    border: none;
    cursor: pointer;
    color: #495057;
    background-color: #e9ecef;
}
.btn:hover { opacity: 0.9; }
.btn-primary { background-color: #007bff; color: white; }
.btn-secondary { background-color: #6c757d; color: white; }
.btn-danger { background-color: #dc3545; color: white; }

.form-styled p { margin-bottom: 15px; }
.form-styled label { display: block; margin-bottom: 5px; font-weight: 600; }
.form-styled input[type="text"], .form-styled input[type="file"] {
    width: 100%;
    padding: 10px;
    border: 1px solid #ced4da;
    border-radius: 4px;
    box-sizing: border-box;
}
```

---

### **🔥 ¡Proyecto Terminado!**

¡Felicidades! Ahora tienes una aplicación web completamente funcional.

**Para probarla:**
1.  Asegúrate de que no haya errores en la terminal.
2.  Ejecuta el servidor: `python manage.py runserver`
3.  Abre tu navegador y ve a `http://127.0.0.1:8000/`.

**Ahora puedes:**
*   Ver la lista de artistas.
*   Hacer clic en un artista para ver sus detalles y su lista de canciones.
*   Crear nuevos artistas con fotos.
*   Editar la información de un artista.
*   Borrar un artista (y sus canciones asociadas).
*   Gestionar todo desde el panel de administración de Django.
