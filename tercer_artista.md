¡Perfecto! Vamos a desarrollar **Clase 2** paso a paso, con **código completo, explicaciones breves, comentarios en cada sección** y una estructura lista para que tus estudiantes de preparatoria puedan crear una app Django profesional, con CRUD completo, rutas, plantillas, formularios, estilos CSS y base.html.

---

# 📘 CLASE 2: Django con Python para implementar aplicaciones web profesionales

📍 **Nivel:** Principiante
📍 **Sistema operativo:** Windows
📍 **Tecnologías:** Python, Django, HTML, CSS
📍 **Objetivo:** Al finalizar, los alumnos podrán:

* Crear un proyecto Django.
* Implementar vistas, modelos y formularios.
* Usar enrutamiento y plantillas.
* Desarrollar un CRUD completo (crear, listar, editar, eliminar) con imágenes.

---

## 1️⃣ Procedimiento paso a paso

### 🔹 Paso 1-12: Configuración del proyecto

```bash
# 1. Crear carpeta de trabajo
mkdir Curso_Django

# 2. Crear subcarpeta "Artista"
cd Curso_Django
mkdir Artista
cd Artista

# 3. Abrir VS Code
code .

# 4. Abrir terminal en VS Code y verificar Python y pip
python --version
pip --version

# 5. Crear entorno virtual
python -m venv .venv

# 6. Activar entorno virtual
.venv\Scripts\activate

# 7. Seleccionar intérprete Python en VS Code (Ctrl+Shift+P > Python: Select Interpreter)

# 8. Instalar Django
pip install django
django-admin --version

# 9. Crear proyecto
django-admin startproject backend_artista .

# 10. Ejecutar servidor integrado
python manage.py runserver

# 11. Abrir navegador en http://127.0.0.1:8000/
# 12. Detener servidor (Ctrl+C)
```

---

### 🔹 Paso 13-17: Migraciones y Superusuario

```bash
# 13. Realizar migraciones iniciales
python manage.py makemigrations

# 14. Migrar base de datos
python manage.py migrate

# 15. Crear superusuario
python manage.py createsuperuser
# (Ingresar usuario, email y contraseña)

# 16. Ejecutar servidor y acceder a /admin
python manage.py runserver

# 17. Detener servidor (Ctrl+C)
```

---

### 🔹 Paso 18-22: Crear aplicación `app_artista`

```bash
# 18. Crear aplicación
python manage.py startapp app_artista
```

📌 **backend\_artista/settings.py** → Registrar app:

```python
INSTALLED_APPS = [
    # Apps Django
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # App personalizada
    'app_artista',
]
```

📌 **backend\_artista/urls.py** → Incluir ruta de `app_artista`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),  # Ruta principal
]
```

📌 **app\_artista/urls.py** (nuevo archivo):

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
]
```

---

### 🔹 Paso 23-24: Modelo `Artista`

📌 **app\_artista/models.py**

```python
from django.db import models

# Modelo Artista con campos básicos y foto
class Artista(models.Model):
    nombre = models.CharField(max_length=100)
    nacionalidad = models.CharField(max_length=100)
    foto_artista = models.ImageField(upload_to='img_artistas/')  # Carpeta de subida

    def __str__(self):
        return self.nombre  # Representación en admin

    class Meta:
        verbose_name = "Artista"
        verbose_name_plural = "Artistas"
```

📌 **app\_artista/admin.py**

```python
from django.contrib import admin
from .models import Artista

admin.site.register(Artista)  # Registrar modelo
```

---

### 🔹 Paso 25-28: Vista y plantilla para listar artistas

📌 Crear carpeta `templates` en `app_artista`:

```
app_artista/
 └── templates/
      └── listar_artistas.html
```

📌 **listar\_artistas.html (inicial)**

```html
<h2>Artistas <a href="">(+)</a></h2>
<ul>
  {% for artista in artistas %}
  <li>
    <a href="">{{ artista.nombre }}</a>
  </li>
  {% endfor %}
</ul>
```

📌 **app\_artista/views.py**

```python
from django.shortcuts import render
from .models import Artista

# Vista para listar artistas
def listar_artistas(request):
    artistas = Artista.objects.all()  # Consulta todos los artistas
    return render(request, 'listar_artistas.html', {'artistas': artistas})
```

```bash
# Migraciones
python manage.py makemigrations
python manage.py migrate
```

---

### 🔹 Paso 37-40: Detalle del artista

📌 **views.py** → nueva función

```python
# NUEVO CÓDIGO: Vista detalle_artista
def detalle_artista(request, artista_id):
    artista = Artista.objects.get(id=artista_id)
    return render(request, 'detalle_artista.html', {'artista': artista})
```

📌 **urls.py**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    # NUEVO CÓDIGO:
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
]
```

📌 **listar\_artistas.html (actualizado)**

```html
<!-- NUEVO CÓDIGO: Enlace dinámico -->
<h2>Artistas <a href="">(+)</a></h2>
<ul>
  {% for artista in artistas %}
  <li>
    <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
  </li>
  {% endfor %}
</ul>
```

📌 **detalle\_artista.html**

```html
<h2>{{ artista.nombre }} <a href="">(edit)</a></h2>
<h4>{{ artista.nacionalidad }}</h4>

<img src="{{ artista.foto_artista.url }}" alt="" class="foto-artista" width="300" height="250" />

<h3>Canciones <a href="">(+)</a></h3>
<ul>
  {% for cancion in artista.canciones.all %}
  <li>{{ cancion.titulo }}</li>
  {% endfor %}
</ul>
```

---

### 🔹 Paso 45-49: Archivos estáticos y base.html

📌 Estructura:

```
app_artista/
 └── static/
      └── css/
           └── styles.css
```

📌 **styles.css**

```css
body {
  font-family: Arial, sans-serif;
  background: #f4f4f4;
  margin: 0;
  padding: 0;
}

h1, h2, h3 {
  color: #333;
  text-align: center;
}

nav {
  background: #222;
  padding: 10px;
  text-align: center;
}

nav a {
  color: white;
  margin: 0 10px;
  text-decoration: none;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 10px;
  padding: 10px;
  background: #fff;
  border-radius: 5px;
}
```

📌 **base.html**

```html
{% load static %}
<html>
  <head>
    <title>Melodia</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}" />
  </head>
  <body>
    <h1>Melodias</h1>
    <nav>
      <a href="/canciones">Canciones</a>
      <a href="/">Artistas</a>
    </nav>
    {% block contenido %}{% endblock %}
  </body>
</html>
```

📌 **listar\_artistas.html (heredando de base.html)**

```html
{% extends 'base.html' %}
{% block contenido %}
<h2>Artistas <a href="">(+)</a></h2>
<ul>
  {% for artista in artistas %}
  <li>
    <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
  </li>
  {% endfor %}
</ul>
{% endblock %}
```

---

### 🔹 Paso 52: Configurar media y static

📌 **settings.py**

```python
import os

STATIC_URL = '/static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'app_artista/static')]
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

📌 **backend\_artista/urls.py**

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

### 🔹 Paso 53: Formulario para Artista

📌 **forms.py**

```python
from django import forms
from .models import Artista

class ArtistaForm(forms.ModelForm):
    class Meta:
        model = Artista
        fields = ['nombre', 'nacionalidad', 'foto_artista']
```

---

### 🔹 Paso 54-58: CRUD completo

📌 **views.py**

```python
from django.shortcuts import render, get_object_or_404, redirect
from .forms import ArtistaForm
from .models import Artista

# Crear artista
def crear_artista(request):
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('listar_artistas')
    else:
        form = ArtistaForm()
    return render(request, 'formulario_artista.html', {'form': form})

# Editar artista
def editar_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES, instance=artista)
        if form.is_valid():
            form.save()
            return redirect('detalle_artista', artista_id=artista.id)
    else:
        form = ArtistaForm(instance=artista)
    return render(request, 'formulario_artista.html', {'form': form})

# Borrar artista
def borrar_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        artista.delete()
        return redirect('listar_artistas')
    return render(request, 'confirmar_borrar.html', {'artista': artista})
```

📌 **urls.py (CRUD agregado)**

```python
urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    # NUEVO CRUD:
    path('crear/', views.crear_artista, name='crear_artista'),
    path('editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    path('borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

📌 **formulario\_artista.html**

```html
{% extends 'base.html' %}
{% block contenido %}
<h2>Formulario Artista</h2>
<form method="post" enctype="multipart/form-data">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Guardar</button>
</form>
{% endblock %}
```

📌 **detalle\_artista.html (con botones)**

```html
{% extends 'base.html' %}
{% block contenido %}
<h2>{{ artista.nombre }}</h2>
<h4>{{ artista.nacionalidad }}</h4>
<img src="{{ artista.foto_artista.url }}" width="300" height="250" />

<p>
  <a href="{% url 'editar_artista' artista.id %}">Editar</a> |
  <a href="{% url 'borrar_artista' artista.id %}">Borrar</a>
</p>
{% endblock %}
```

---

## 📂 Estructura final de carpetas

```
Curso_Django/
 └── Artista/
      ├── backend_artista/
      │    ├── settings.py
      │    ├── urls.py
      ├── app_artista/
      │    ├── admin.py
      │    ├── forms.py
      │    ├── models.py
      │    ├── urls.py
      │    ├── views.py
      │    ├── static/css/styles.css
      │    └── templates/
      │         ├── base.html
      │         ├── listar_artistas.html
      │         ├── detalle_artista.html
      │         ├── formulario_artista.html
      └── manage.py
```

---

¿Quieres que ahora **genere el archivo `confirmar_borrar.html`** con estilo para la opción de borrar artista y que quede listo?
