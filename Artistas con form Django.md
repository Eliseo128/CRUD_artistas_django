# Proyecto Django para Gestión de Artistas y Canciones

Voy a guiarte paso a paso en la creación de un proyecto Django completo para gestionar artistas y canciones. Este proyecto incluirá todas las funcionalidades CRUD y una interfaz atractiva.

## Estructura inicial del proyecto

Primero, vamos a crear la estructura de carpetas y archivos necesarios:

```
Curso_Django/
└── Artista/
    ├── backend_artista/
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── app_artista/
    │   ├── migrations/
    │   │   └── __init__.py
    │   ├── static/
    │   │   └── css/
    │   │       └── styles.css
    │   ├── templates/
    │   │   ├── base.html
    │   │   ├── listar_artistas.html
    │   │   ├── detalle_artista.html
    │   │   └── formulario_artista.html
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── forms.py
    │   ├── models.py
    │   ├── tests.py
    │   ├── urls.py
    │   └── views.py
    ├── media/
    │   └── img_artistas/
    ├── .venv/
    └── manage.py
```

## Paso a paso detallado

### 1. Configuración inicial

Abre la terminal de VS Code y ejecuta los siguientes comandos:

```bash
# Verificar versiones
python --version
pip --version

# Crear entorno virtual
python -m venv .venv

# Activar entorno virtual (Windows)
.venv\Scripts\activate

# Seleccionar intérprete de Python (Ctrl+Shift+P -> Python: Select Interpreter -> .venv)
# Instalar Django
pip install django

# Verificar instalación
python -m django --version
```

### 2. Crear el proyecto y la aplicación

```bash
# Crear proyecto
django-admin startproject backend_artista .

# Crear aplicación
python manage.py startapp app_artista
```

### 3. Configurar settings.py

Abre `backend_artista/settings.py` y agrega:

```python
# backend_artista/settings.py

import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

# Agregar la aplicación a INSTALLED_APPS
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_artista',  # Nueva aplicación
]

# Configuración para archivos estáticos y medios
STATIC_URL = '/static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'app_artista/static')]

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

### 4. Definir los modelos

En `app_artista/models.py`:

```python
from django.db import models

class Artista(models.Model):
    nombre = models.CharField(max_length=100)
    nacionalidad = models.CharField(max_length=100)
    foto_artista = models.ImageField(upload_to='img_artistas/', blank=True, null=True)
    
    def __str__(self):
        return self.nombre
    
    class Meta:
        verbose_name = "Artista"
        verbose_name_plural = "Artistas"

class Cancion(models.Model):
    titulo = models.CharField(max_length=100)
    artista = models.ForeignKey(Artista, on_delete=models.CASCADE, related_name='canciones')
    
    def __str__(self):
        return f"{self.titulo} - {self.artista.nombre}"
    
    class Meta:
        verbose_name = "Canción"
        verbose_name_plural = "Canciones"
```

### 5. Configurar el admin

En `app_artista/admin.py`:

```python
from django.contrib import admin
from .models import Artista, Cancion

@admin.register(Artista)
class ArtistaAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'nacionalidad')
    search_fields = ('nombre', 'nacionalidad')

@admin.register(Cancion)
class CancionAdmin(admin.ModelAdmin):
    list_display = ('titulo', 'artista')
    list_filter = ('artista',)
    search_fields = ('titulo', 'artista__nombre')
```

### 6. Crear formularios

En `app_artista/forms.py`:

```python
from django import forms
from .models import Artista, Cancion

class ArtistaForm(forms.ModelForm):
    class Meta:
        model = Artista
        fields = ['nombre', 'nacionalidad', 'foto_artista']
        widgets = {
            'nombre': forms.TextInput(attrs={'class': 'form-control'}),
            'nacionalidad': forms.TextInput(attrs={'class': 'form-control'}),
            'foto_artista': forms.FileInput(attrs={'class': 'form-control'}),
        }

class CancionForm(forms.ModelForm):
    class Meta:
        model = Cancion
        fields = ['titulo', 'artista']
        widgets = {
            'titulo': forms.TextInput(attrs={'class': 'form-control'}),
            'artista': forms.Select(attrs={'class': 'form-control'}),
        }
```

### 7. Crear las vistas

En `app_artista/views.py`:

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Artista, Cancion
from .forms import ArtistaForm, CancionForm

def listar_artistas(request):
    """Vista para listar todos los artistas"""
    artistas = Artista.objects.all()
    return render(request, 'listar_artistas.html', {'artistas': artistas})

def detalle_artista(request, artista_id):
    """Vista para mostrar los detalles de un artista específico"""
    artista = get_object_or_404(Artista, id=artista_id)
    return render(request, 'detalle_artista.html', {'artista': artista})

def crear_artista(request):
    """Vista para crear un nuevo artista"""
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('listar_artistas')
    else:
        form = ArtistaForm()
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Crear Artista'})

def editar_artista(request, artista_id):
    """Vista para editar un artista existente"""
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES, instance=artista)
        if form.is_valid():
            form.save()
            return redirect('detalle_artista', artista_id=artista.id)
    else:
        form = ArtistaForm(instance=artista)
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Editar Artista'})

def borrar_artista(request, artista_id):
    """Vista para eliminar un artista"""
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        artista.delete()
        return redirect('listar_artistas')
    return render(request, 'confirmar_borrado.html', {'artista': artista})
```

### 8. Configurar las URLs

En `backend_artista/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),
]

# Servir archivos multimedia durante el desarrollo
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

En `app_artista/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    path('artista/crear/', views.crear_artista, name='crear_artista'),
    path('artista/editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    path('artista/borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

### 9. Crear las plantillas

`app_artista/templates/base.html`:

```html
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Melodía - {% block title %}Gestión de Artistas{% endblock %}</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}" />
</head>
<body>
    <header>
        <h1>Melodías</h1>
        <nav>
            <a href="{% url 'listar_artistas' %}">Artistas</a>
            <a href="/admin">Administración</a>
        </nav>
    </header>
    
    <main>
        {% block contenido %}
        {% endblock %}
    </main>
    
    <footer>
        <p>&copy; 2023 Curso Django - Sistema de Gestión de Artistas</p>
    </footer>
</body>
</html>
```

`app_artista/templates/listar_artistas.html`:

```html
{% extends 'base.html' %}
{% load static %}

{% block title %}Lista de Artistas{% endblock %}

{% block contenido %}
<div class="container">
    <h2>Artistas <a href="{% url 'crear_artista' %}" class="btn-add">(+)</a></h2>
    
    {% if artistas %}
    <ul class="artist-list">
        {% for artista in artistas %}
        <li class="artist-item">
            <div class="artist-info">
                {% if artista.foto_artista %}
                <img src="{{ artista.foto_artista.url }}" alt="{{ artista.nombre }}" class="artist-thumb">
                {% else %}
                <img src="{% static 'img/placeholder.jpg' %}" alt="Sin imagen" class="artist-thumb">
                {% endif %}
                <a href="{% url 'detalle_artista' artista.id %}" class="artist-name">{{ artista.nombre }}</a>
            </div>
            <div class="artist-actions">
                <a href="{% url 'editar_artista' artista.id %}" class="btn-edit">Editar</a>
                <a href="{% url 'borrar_artista' artista.id %}" class="btn-delete">Eliminar</a>
            </div>
        </li>
        {% endfor %}
    </ul>
    {% else %}
    <p class="no-data">No hay artistas registrados todavía.</p>
    {% endif %}
</div>
{% endblock %}
```

`app_artista/templates/detalle_artista.html`:

```html
{% extends 'base.html' %}
{% load static %}

{% block title %}{{ artista.nombre }}{% endblock %}

{% block contenido %}
<div class="container">
    <div class="artist-header">
        <h2>{{ artista.nombre }} <a href="{% url 'editar_artista' artista.id %}" class="btn-edit">(editar)</a></h2>
        <h4>{{ artista.nacionalidad }}</h4>
    </div>
    
    <div class="artist-detail">
        <div class="artist-image">
            {% if artista.foto_artista %}
            <img src="{{ artista.foto_artista.url }}" alt="{{ artista.nombre }}" class="artist-photo">
            {% else %}
            <img src="{% static 'img/placeholder.jpg' %}" alt="Sin imagen" class="artist-photo">
            {% endif %}
        </div>
        
        <div class="artist-songs">
            <h3>Canciones</h3>
            
            {% if artista.canciones.all %}
            <ul class="song-list">
                {% for cancion in artista.canciones.all %}
                <li class="song-item">
                    <span class="song-title">{{ cancion.titulo }}</span>
                </li>
                {% endfor %}
            </ul>
            {% else %}
            <p class="no-data">Este artista no tiene canciones registradas.</p>
            {% endif %}
        </div>
    </div>
    
    <div class="actions">
        <a href="{% url 'listar_artistas' %}" class="btn-back">Volver a la lista</a>
        <a href="{% url 'borrar_artista' artista.id %}" class="btn-delete">Eliminar artista</a>
    </div>
</div>
{% endblock %}
```

`app_artista/templates/formulario_artista.html`:

```html
{% extends 'base.html' %}
{% load static %}

{% block title %}{{ titulo }}{% endblock %}

{% block contenido %}
<div class="container">
    <h2>{{ titulo }}</h2>
    
    <form method="post" enctype="multipart/form-data" class="artist-form">
        {% csrf_token %}
        
        <div class="form-group">
            <label for="{{ form.nombre.id_for_label }}">Nombre:</label>
            {{ form.nombre }}
            {% if form.nombre.errors %}
            <div class="error">{{ form.nombre.errors }}</div>
            {% endif %}
        </div>
        
        <div class="form-group">
            <label for="{{ form.nacionalidad.id_for_label }}">Nacionalidad:</label>
            {{ form.nacionalidad }}
            {% if form.nacionalidad.errors %}
            <div class="error">{{ form.nacionalidad.errors }}</div>
            {% endif %}
        </div>
        
        <div class="form-group">
            <label for="{{ form.foto_artista.id_for_label }}">Foto:</label>
            {{ form.foto_artista }}
            {% if form.foto_artista.errors %}
            <div class="error">{{ form.foto_artista.errors }}</div>
            {% endif %}
            
            {% if form.instance.foto_artista %}
            <div class="current-image">
                <p>Imagen actual:</p>
                <img src="{{ form.instance.foto_artista.url }}" alt="Imagen actual" class="current-img">
            </div>
            {% endif %}
        </div>
        
        <div class="form-actions">
            <button type="submit" class="btn-save">Guardar</button>
            <a href="{% url 'listar_artistas' %}" class="btn-cancel">Cancelar</a>
        </div>
    </form>
</div>
{% endblock %}
```

### 10. Crear los estilos CSS

En `app_artista/static/css/styles.css`:

```css
/* Estilos generales */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
    background-color: #f5f5f5;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

/* Header */
header {
    background-color: #2c3e50;
    color: white;
    padding: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header h1 {
    font-size: 2rem;
}

nav a {
    color: white;
    text-decoration: none;
    margin-left: 20px;
    padding: 5px 10px;
    border-radius: 3px;
    transition: background-color 0.3s;
}

nav a:hover {
    background-color: #34495e;
}

/* Lista de artistas */
.artist-list {
    list-style: none;
    margin-top: 20px;
}

.artist-item {
    background: white;
    border-radius: 5px;
    padding: 15px;
    margin-bottom: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.artist-info {
    display: flex;
    align-items: center;
}

.artist-thumb {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    object-fit: cover;
    margin-right: 15px;
}

.artist-name {
    font-size: 1.2rem;
    color: #2c3e50;
    text-decoration: none;
    font-weight: bold;
}

.artist-name:hover {
    color: #3498db;
}

.artist-actions {
    display: flex;
    gap: 10px;
}

/* Detalles del artista */
.artist-header {
    background: white;
    padding: 20px;
    border-radius: 5px;
    margin-bottom: 20px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.artist-detail {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 30px;
    margin-bottom: 20px;
}

.artist-image {
    background: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.artist-photo {
    width: 100%;
    border-radius: 5px;
}

.artist-songs {
    background: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.song-list {
    list-style: none;
    margin-top: 15px;
}

.song-item {
    padding: 10px;
    border-bottom: 1px solid #eee;
}

.song-item:last-child {
    border-bottom: none;
}

.song-title {
    font-weight: 500;
}

/* Formularios */
.artist-form {
    background: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
}

.form-group input,
.form-group select {
    width: 100%;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 1rem;
}

.current-image {
    margin-top: 10px;
}

.current-img {
    max-width: 200px;
    border-radius: 5px;
    margin-top: 10px;
}

.form-actions {
    display: flex;
    gap: 10px;
    margin-top: 20px;
}

/* Botones */
.btn-add, .btn-edit, .btn-delete, .btn-save, .btn-cancel, .btn-back {
    display: inline-block;
    padding: 8px 15px;
    border-radius: 4px;
    text-decoration: none;
    font-weight: 500;
    cursor: pointer;
    border: none;
    font-size: 0.9rem;
}

.btn-add {
    background-color: #27ae60;
    color: white;
}

.btn-edit {
    background-color: #3498db;
    color: white;
}

.btn-delete {
    background-color: #e74c3c;
    color: white;
}

.btn-save {
    background-color: #27ae60;
    color: white;
}

.btn-cancel, .btn-back {
    background-color: #95a5a6;
    color: white;
}

.btn-add:hover {
    background-color: #219653;
}

.btn-edit:hover {
    background-color: #2980b9;
}

.btn-delete:hover {
    background-color: #c0392b;
}

.btn-save:hover {
    background-color: #219653;
}

.btn-cancel:hover, .btn-back:hover {
    background-color: #7f8c8d;
}

/* Mensajes y estados */
.no-data {
    text-align: center;
    padding: 20px;
    background: white;
    border-radius: 5px;
    color: #7f8c8d;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.error {
    color: #e74c3c;
    font-size: 0.9rem;
    margin-top: 5px;
}

/* Footer */
footer {
    text-align: center;
    padding: 20px;
    margin-top: 40px;
    background-color: #2c3e50;
    color: white;
}

/* Responsive */
@media (max-width: 768px) {
    .artist-detail {
        grid-template-columns: 1fr;
    }
    
    .artist-item {
        flex-direction: column;
        align-items: flex-start;
    }
    
    .artist-actions {
        margin-top: 15px;
        width: 100%;
        justify-content: space-between;
    }
    
    header {
        flex-direction: column;
        text-align: center;
    }
    
    nav {
        margin-top: 15px;
    }
    
    nav a {
        margin: 0 5px;
    }
}
```

### 11. Realizar migraciones y ejecutar el servidor

```bash
# Realizar migraciones
python manage.py makemigrations
python manage.py migrate

# Crear superusuario
python manage.py createsuperuser

# Ejecutar servidor
python manage.py runserver
```

## Extensiones recomendadas para VS Code

1. Python (Microsoft)
2. Django (Baptiste Darthenay)
3. HTML CSS Support (ecmel)
4. Auto Rename Tag (formulahendry)
5. Bracket Pair Colorizer (CoenraadS)
6. GitLens (Eric Amodio)
7. GitHub Copilot (GitHub)
8. SQLite (alexcvzz)

## Visor de base de datos recomendado

Para SQLite, recomiendo DB Browser for SQLite (https://sqlitebrowser.org/), que es gratuito y fácil de usar.

## Explicación del proyecto

Este proyecto implementa un sistema CRUD completo para gestionar artistas y sus canciones. Incluye:

1. **Modelos**: Artista y Canción con relación uno a muchos
2. **Vistas**: Listar, detalle, crear, editar y eliminar artistas
3. **Formularios**: Para crear y editar artistas con validación
4. **Plantillas**: Con herencia para reutilizar código
5. **Archivos estáticos**: CSS para una interfaz atractiva
6. **Panel de administración**: Para gestión avanzada
7. **Manejo de imágenes**: Para las fotos de artistas

El diseño es responsive y funciona bien en dispositivos móviles y desktop. Los estudiantes pueden agregar más funcionalidades como búsqueda, paginación, o un sistema de usuarios.
