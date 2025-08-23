# CRUD_artistas_django
el crud proyecto
```markdown
# 🎓 Clase 2: Django con Python para Implementar Aplicaciones Web Profesionales

**Nivel:** Principiante  
**Escolaridad:** Preparatoria  
**Sistema Operativo:** Windows  
**Duración estimada:** 3-4 horas  

---

## 🎯 Objetivos de Aprendizaje

Al finalizar este tutorial, los estudiantes podrán:
- Crear un proyecto completo con **Django**
- Definir **modelos**, **vistas**, **formularios** y **plantillas**
- Usar **enrutamiento** para navegar entre páginas
- Implementar funcionalidad **CRUD completa** (Crear, Leer, Actualizar, Eliminar)
- Trabajar con el **panel de administración** de Django
- Usar **archivos estáticos (CSS)** y **plantillas base**
- Visualizar datos con **HTML** y **estilos personalizados**

---

## 🛠️ Tecnologías y Herramientas

| Herramienta | Propósito |
|-----------|---------|
| **Python** | Lenguaje principal |
| **Django** | Framework web |
| **HTML** | Estructura de páginas |
| **CSS** | Estilos visuales |
| **VS Code** | Editor de código |
| **SQLite** | Base de datos (integrada en Django) |

### ✅ Extensiones recomendadas en VS Code:
- Python
- Django
- HTML CSS Support
- GitHub Copilot *(opcional, pero muy útil)*

### 🔍 Visor de base de datos recomendado:
👉 **[DB Browser for SQLite](https://sqlitebrowser.org/)** (gratuito, fácil de usar, ideal para principiantes)

---

## 📁 Estructura Final del Proyecto

```
Curso_Django/
└── Artista/
    ├── .venv/                  # Entorno virtual
    ├── backend_artista/         # Proyecto Django
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   ├── wsgi.py
    │   └── asgi.py
    ├── app_artista/             # Aplicación principal
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── models.py
    │   ├── views.py
    │   ├── forms.py
    │   ├── urls.py
    │   ├── templates/
    │   │   ├── base.html
    │   │   ├── listar_artistas.html
    │   │   ├── detalle_artista.html
    │   │   └── formulario_artista.html
    │   └── static/
    │       └── css/
    │           └── styles.css
    ├── manage.py
    └── requirements.txt         # Opcional: lista de dependencias
```

---

## 🔧 Procedimiento Paso a Paso

---

### ✅ Paso 1: Crear carpeta de trabajo

Abre el Explorador de Windows y crea:
```
Curso_Django\Artista
```

---

### ✅ Paso 2: Abrir VS Code

Abre la carpeta `Artista` con **Visual Studio Code**:
- Botón derecho sobre la carpeta → "Abrir con Code"

---

### ✅ Paso 3: Verificar Python y pip

Abre una **nueva terminal** en VS Code (`Terminal > Nuevo terminal`) y ejecuta:

```bash
python --version
pip --version
```

> ✅ Debe mostrar algo como:  
> `Python 3.11.x` y `pip 23.x.x`

---

### ✅ Paso 4: Crear entorno virtual

En la terminal:

```bash
python -m venv .venv
```

Esto crea una carpeta `.venv` con tu entorno aislado.

---

### ✅ Paso 5: Activar entorno virtual

```bash
.venv\Scripts\activate
```

> 🔹 Verás que el prompt cambia a: `(.venv) C:\...>`

---

### ✅ Paso 6: Seleccionar intérprete de Python

Presiona `Ctrl + Shift + P` → Escribe: **"Python: Select Interpreter"**  
→ Elige el que diga: `.venv\Scripts\python.exe`

---

### ✅ Paso 7: Instalar Django

```bash
pip install django
```

Verifica la instalación:

```bash
python -m django --version
```

> ✅ Debe mostrar: `4.2.x` o similar

---

### ✅ Paso 8: Crear el proyecto Django

```bash
django-admin startproject backend_artista .
```

> 🔹 El punto `.` es importante: crea los archivos en la carpeta actual.

---

### ✅ Paso 9: Ejecutar el servidor de desarrollo

```bash
python manage.py runserver
```

Abre tu navegador y ve a: [http://127.0.0.1:8000](http://127.0.0.1:8000)

> ✅ Deberás ver la página de bienvenida de Django: "The install worked successfully!"

---

### ✅ Paso 10: Detener el servidor

Presiona `Ctrl + C` en la terminal para detener el servidor.

---

### ✅ Paso 11: Realizar migraciones iniciales

```bash
python manage.py makemigrations
python manage.py migrate
```

> 🔹 Las migraciones crean las tablas iniciales en la base de datos (usuarios, sesiones, etc.)

---

### ✅ Paso 12: Crear superusuario

```bash
python manage.py createsuperuser
```

Ingresa:
- **Username:** admin
- **Email:** (opcional)
- **Password:** 12345678 (mínimo 8 caracteres)
- Confirma la contraseña

> ⚠️ No uses esta contraseña en producción. Solo para desarrollo.

---

### ✅ Paso 13: Iniciar servidor y acceder al admin

```bash
python manage.py runserver
```

Ve a: [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)  
Inicia sesión con el usuario creado.

> ✅ Deberás ver el panel de administración con "Groups" y "Users".

---

### ✅ Paso 14: Crear la aplicación `app_artista`

Detén el servidor (`Ctrl + C`) y ejecuta:

```bash
python manage.py startapp app_artista
```

---

### ✅ Paso 15: Registrar la app en `settings.py`

Abre: `backend_artista/settings.py`

Busca `INSTALLED_APPS` y agrega:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_artista',  # ← Agrega esta línea
]
```

---

### ✅ Paso 16: Crear `urls.py` en `app_artista`

Crea el archivo: `app_artista/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
]
```

---

### ✅ Paso 17: Configurar rutas en el proyecto

Abre: `backend_artista/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),  # ← Nueva ruta
]
```

---

### ✅ Paso 18: Crear el modelo `Artista`

Abre: `app_artista/models.py`

```python
from django.db import models

class Artista(models.Model):
    nombre = models.CharField(max_length=100)
    nacionalidad = models.CharField(max_length=50)
    foto_artista = models.ImageField(upload_to='img_artistas/', blank=True, null=True)

    def __str__(self):
        return self.nombre

    class Meta:
        verbose_name = "Artista"
        verbose_name_plural = "Artistas"
```

---

### ✅ Paso 19: Registrar modelo en el admin

Abre: `app_artista/admin.py`

```python
from django.contrib import admin
from .models import Artista

admin.site.register(Artista)
```

---

### ✅ Paso 20: Hacer migraciones del modelo

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### ✅ Paso 21: Crear carpeta de plantillas

Dentro de `app_artista`, crea:
```
templates/
```

---

### ✅ Paso 22: Crear `listar_artistas.html`

Crea: `app_artista/templates/listar_artistas.html`

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

---

### ✅ Paso 23: Crear vista `listar_artistas`

Abre: `app_artista/views.py`

```python
from django.shortcuts import render
from .models import Artista

def listar_artistas(request):
    artistas = Artista.objects.all()
    return render(request, 'listar_artistas.html', {'artistas': artistas})
```

---

### ✅ Paso 24: Reiniciar servidor y probar

```bash
python manage.py runserver
```

Ve a: [http://127.0.0.1:8000](http://127.0.0.1:8000)  
> ✅ Deberías ver la lista vacía de artistas.

---

### ✅ Paso 25: Agregar artistas desde el admin

1. Ve a [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)
2. Inicia sesión
3. Haz clic en "Artistas" → "Añadir"
4. Agrega 3 artistas (ej: "Juanes", "Shakira", "Carlos Vives")
5. Sube una foto para cada uno (opcional por ahora)

---

### ✅ Paso 26: Crear vista `detalle_artista`

En `app_artista/views.py`, añade:

```python
def detalle_artista(request, artista_id):
    artista = Artista.objects.get(id=artista_id)
    return render(request, 'detalle_artista.html', {'artista': artista})
```

---

### ✅ Paso 27: Agregar ruta para detalle

Actualiza `app_artista/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
]
```

---

### ✅ Paso 28: Actualizar enlace en `listar_artistas.html`

Modifica el archivo:

```html
<h2>Artistas <a href="">(+)</a></h2>
<ul>
  {% for artista in artistas %}
  <li>
    <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
  </li>
  {% endfor %}
</ul>
```

---

### ✅ Paso 29: Crear `detalle_artista.html`

Crea: `app_artista/templates/detalle_artista.html`

```html
<h2>{{ artista.nombre }} <a href="">(edit)</a></h2>
<h4>{{ artista.nacionalidad }}</h4>

<img src="{{ artista.foto_artista.url }}" alt="{{ artista.nombre }}" class="foto-artista" width="300" height="250" />

<h3>Canciones <a href="">(+)</a></h3>
<ul>
  {% for cancion in artista.canciones.all %}
  <li>
    <a href="">{{ cancion.titulo }}</a>
  </li>
  {% endfor %}
</ul>
```

> 🔹 Nota: `artista.foto_artista.url` es necesario para imágenes.

---

### ✅ Paso 30: Crear carpetas para archivos estáticos

Dentro de `app_artista`:
```
static/
└── css/
    └── styles.css
```

---

### ✅ Paso 31: Crear `styles.css`

`app_artista/static/css/styles.css`:

```css
body {
  font-family: 'Segoe UI', sans-serif;
  background: #f0f2f5;
  margin: 0;
  padding: 20px;
}

h1 {
  color: #4a5568;
  text-align: center;
}

nav {
  text-align: center;
  margin: 20px 0;
}

nav a {
  margin: 0 15px;
  text-decoration: none;
  color: #667eea;
  font-weight: bold;
}

nav a:hover {
  text-decoration: underline;
}

.foto-artista {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

ul {
  list-style: none;
  padding: 0;
}

ul li {
  margin: 10px 0;
}

ul li a {
  text-decoration: none;
  color: #2d3748;
  font-size: 18px;
}

ul li a:hover {
  color: #667eea;
}
```

---

### ✅ Paso 32: Crear `base.html`

`app_artista/templates/base.html`:

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
    <a href="/canciones">Canciones</a>
    <a href="/">Artistas</a>
  </nav>
  {% block contenido %}{% endblock %}
</body>
</html>
```

---

### ✅ Paso 33: Usar plantilla base

Actualiza `listar_artistas.html`:

```html
{% extends "base.html" %}
{% load static %}

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

Haz lo mismo con `detalle_artista.html`:

```html
{% extends "base.html" %}
{% load static %}

{% block contenido %}
<h2>{{ artista.nombre }} <a href="">(edit)</a></h2>
<h4>{{ artista.nacionalidad }}</h4>

<img src="{{ artista.foto_artista.url }}" alt="{{ artista.nombre }}" class="foto-artista" />

<h3>Canciones <a href="">(+)</a></h3>
<ul>
  {% for cancion in artista.canciones.all %}
  <li>
    <a href="">{{ cancion.titulo }}</a>
  </li>
  {% endfor %}
</ul>
{% endblock %}
```

---

### ✅ Paso 34: Configurar archivos estáticos

En `backend_artista/settings.py`, asegúrate de tener:

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / "app_artista" / "static",
]
```

Y en `backend_artista/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

También agrega en `settings.py`:

```python
import os
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

---

### ✅ Paso 35: Crear `forms.py`

`app_artista/forms.py`:

```python
from django import forms
from .models import Artista

class ArtistaForm(forms.ModelForm):
    class Meta:
        model = Artista
        fields = ['nombre', 'nacionalidad', 'foto_artista']
        widgets = {
            'nombre': forms.TextInput(attrs={'class': 'form-control'}),
            'nacionalidad': forms.TextInput(attrs={'class': 'form-control'}),
        }
```

---

### ✅ Paso 36: Agregar vistas CRUD

En `views.py`:

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Artista
from .forms import ArtistaForm

# ... (las vistas anteriores)

def crear_artista(request):
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('listar_artistas')
    else:
        form = ArtistaForm()
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Crear Artista'})

def editar_artista(request, artista_id):
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
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        artista.delete()
        return redirect('listar_artistas')
    return render(request, 'confirmar_borrar.html', {'artista': artista})
```

---

### ✅ Paso 37: Actualizar `urls.py`

```python
urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    path('crear/', views.crear_artista, name='crear_artista'),
    path('editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    path('borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

---

### ✅ Paso 38: Crear `formulario_artista.html`

```html
{% extends "base.html" %}
{% load static %}

{% block contenido %}
<h2>{{ titulo }}</h2>
<form method="post" enctype="multipart/form-data">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Guardar</button>
  <a href="/">Cancelar</a>
</form>
{% endblock %}
```

---

### ✅ Paso 39: Mejorar `detalle_artista.html` con botones

```html
{% extends "base.html" %}
{% load static %}

{% block contenido %}
<h2>
  {{ artista.nombre }}
  <a href="{% url 'editar_artista' artista.id %}" style="font-size: 16px;">(editar)</a>
</h2>
<h4>{{ artista.nacionalidad }}</h4>

<img src="{{ artista.foto_artista.url }}" alt="{{ artista.nombre }}" class="foto-artista" />

<h3>Canciones <a href="">(+)</a></h3>
<ul>
  {% for cancion in artista.canciones.all %}
  <li><a href="">{{ cancion.titulo }}</a></li>
  {% endfor %}
</ul>

<form method="post" action="{% url 'borrar_artista' artista.id %}" style="margin-top: 20px;">
  {% csrf_token %}
  <button type="submit" style="background: #e53e3e; color: white; border: none; padding: 8px 12px; border-radius: 4px;">Borrar Artista</button>
</form>
{% endblock %}
```

---

### ✅ Paso 40: Ejecutar y probar todo

```bash
python manage.py runserver
```

Prueba:
- Listar artistas
- Ver detalles
- Crear, editar y borrar artistas
- Estilos CSS aplicados
- Imágenes visibles

---

## 🎉 ¡Felicidades! Has completado el proyecto

Este proyecto es **totalmente funcional**, con:
- CRUD completo
- Panel de administración
- Estilos atractivos
- Plantillas reutilizables
- Imágenes y rutas

---

## 📚 Consejos Finales

- Usa **GitHub Copilot** para autocompletar código
- Inspecciona la página con **F12** para ver errores
- Usa **DB Browser for SQLite** para ver la base de datos
- Guarda tus cambios con `git` si aprendes control de versiones

---

📌 **Próximos pasos sugeridos:**
1. Agregar modelo `Cancion` con relación a `Artista`
2. Crear vistas para canciones
3. Usar Bootstrap para mejorar diseño
4. Desplegar en PythonAnywhere o Render

¡Sigue aprendiendo! 🚀
```
