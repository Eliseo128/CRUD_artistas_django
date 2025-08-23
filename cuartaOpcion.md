¡Perfecto! Ahora incluimos **la clase `Cancion`** en el modelo y replanteamos toda la respuesta para que el proyecto quede **completo, funcional y claro**.
Tendrás **dos modelos: `Artista` y `Cancion`** (con relación 1\:N), CRUD de Artistas, vistas para listar y ver detalle del artista (con sus canciones) y todo lo necesario para que los estudiantes de preparatoria lo sigan paso a paso.

---

# 📘 CLASE 2: Django con Python para implementar aplicaciones web profesionales

📍 **Nivel:** Principiante
📍 **Sistema operativo:** Windows
📍 **Tecnologías:** Python, Django, HTML, CSS
📍 **Objetivo:**

* Crear proyecto y aplicación Django.
* Definir modelos `Artista` y `Cancion`.
* Configurar administración de Django.
* Desarrollar vistas y plantillas.
* CRUD completo de artistas.
* Mostrar canciones asociadas a cada artista.

---

## 🔹 1. Procedimiento paso a paso

```bash
# Crear carpeta de trabajo
mkdir Curso_Django
cd Curso_Django

# Crear carpeta Artista
mkdir Artista
cd Artista

# Abrir VS Code
code .

# Verificar versiones
python --version
pip --version

# Crear entorno virtual
python -m venv .venv
.venv\Scripts\activate

# Instalar Django
pip install django

# Crear proyecto
django-admin startproject backend_artista .

# Crear app
python manage.py startapp app_artista
```

---

## 🔹 2. Configurar proyecto y app

📌 **backend\_artista/settings.py**

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # App personalizada
    'app_artista',
]

import os

STATIC_URL = '/static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'app_artista/static')]
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

📌 **backend\_artista/urls.py**

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

📌 **app\_artista/urls.py**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    path('crear/', views.crear_artista, name='crear_artista'),
    path('editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    path('borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

---

## 🔹 3. Modelos: `Artista` y `Cancion`

📌 **app\_artista/models.py**

```python
from django.db import models

# Modelo Artista
class Artista(models.Model):
    nombre = models.CharField(max_length=100)
    nacionalidad = models.CharField(max_length=100)
    foto_artista = models.ImageField(upload_to='img_artistas/')  # Carpeta de imágenes

    def __str__(self):
        return self.nombre

    class Meta:
        verbose_name = "Artista"
        verbose_name_plural = "Artistas"


# Modelo Cancion (relación 1:N con Artista)
class Cancion(models.Model):
    titulo = models.CharField(max_length=100)
    artista = models.ForeignKey(
        Artista, on_delete=models.CASCADE, related_name='canciones'
    )

    def __str__(self):
        return self.titulo

    class Meta:
        verbose_name = "Canción"
        verbose_name_plural = "Canciones"
```

---

## 🔹 4. Registrar modelos en admin

📌 **app\_artista/admin.py**

```python
from django.contrib import admin
from .models import Artista, Cancion

admin.site.register(Artista)
admin.site.register(Cancion)
```

---

## 🔹 5. Formularios

📌 **app\_artista/forms.py**

```python
from django import forms
from .models import Artista

class ArtistaForm(forms.ModelForm):
    class Meta:
        model = Artista
        fields = ['nombre', 'nacionalidad', 'foto_artista']
```

---

## 🔹 6. Vistas con CRUD

📌 **app\_artista/views.py**

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Artista
from .forms import ArtistaForm

# Listar artistas
def listar_artistas(request):
    artistas = Artista.objects.all()
    return render(request, 'listar_artistas.html', {'artistas': artistas})

# Detalle artista
def detalle_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    return render(request, 'detalle_artista.html', {'artista': artista})

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

---

## 🔹 7. Plantillas HTML

📌 **base.html**

```html
{% load static %}
<html>
  <head>
    <title>Melodia</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}" />
  </head>
  <body>
    <h1>Melodías</h1>
    <nav>
      <a href="/">Artistas</a>
    </nav>
    {% block contenido %}{% endblock %}
  </body>
</html>
```

📌 **listar\_artistas.html**

```html
{% extends 'base.html' %}
{% block contenido %}
<h2>Artistas <a href="{% url 'crear_artista' %}">(+)</a></h2>
<ul>
  {% for artista in artistas %}
  <li>
    <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
  </li>
  {% empty %}
  <li>No hay artistas registrados</li>
  {% endfor %}
</ul>
{% endblock %}
```

📌 **detalle\_artista.html**

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

<h3>Canciones</h3>
<ul>
  {% for cancion in artista.canciones.all %}
  <li>{{ cancion.titulo }}</li>
  {% empty %}
  <li>No hay canciones registradas</li>
  {% endfor %}
</ul>
{% endblock %}
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

📌 **confirmar\_borrar.html**

```html
{% extends 'base.html' %}
{% block contenido %}
<h2>¿Seguro que quieres borrar a {{ artista.nombre }}?</h2>
<form method="post">
  {% csrf_token %}
  <button type="submit">Confirmar</button>
  <a href="{% url 'detalle_artista' artista.id %}">Cancelar</a>
</form>
{% endblock %}
```

---

## 🔹 8. Estilos CSS

📌 **app\_artista/static/css/styles.css**

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
  text-align: center;
}

button {
  background: #333;
  color: white;
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
}
```

---

## 📂 Estructura final del proyecto

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
      │         └── confirmar_borrar.html
      └── manage.py
```

---

## 🔥 Resultado final

✅ **CRUD completo de Artistas** (Crear, Leer, Editar, Borrar).
✅ **Relación Artista-Canciones** (se listan canciones en detalle del artista).
✅ **Subida de imágenes**.
✅ **Panel Admin** para gestión rápida.
✅ **Estilo CSS limpio y atractivo**.

---

¿Quieres que **también incluya CRUD de canciones** (crear, editar y eliminar canciones por artista) para que el proyecto quede 100% completo?
