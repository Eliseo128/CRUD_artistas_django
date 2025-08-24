# Secuencia Didáctica: Desarrollo Web con Django

## Información General
- **Duración total**: 15 horas
- **Nivel**: Principiantes (Preparatoria)
- **Tecnologías**: Python, Django, HTML, CSS
- **Software requerido**: Python, VS Code

## Distribución de Tiempo
- **Apertura**: 3 horas
- **Desarrollo**: 9 horas  
- **Cierre**: 3 horas

---

## Momento 1: Apertura (3 horas)

### Objetivo específico:
Introducir los conceptos básicos de Django y configurar el entorno de desarrollo.

### Actividades:

#### 1. Presentación teórica (1 hora)
- Explicación de framework Django y sus componentes (30 min)
- Demostración de una aplicación Django funcional (15 min)
- Preguntas y respuestas (15 min)

#### 2. Configuración del entorno (1.5 horas)
- **Demostrativa**: Instalación y configuración de Python, Django y VS Code (30 min)
- **Autónoma**: Los estudiantes configuran su entorno guiados por el instructor (60 min)

#### 3. Creación del primer proyecto (30 min)
- **Demostrativa**: Creación de proyecto y aplicación Django (15 min)
- **Autónoma**: Los estudiantes crean su primer "Hola Mundo" (15 min)

---

## Momento 2: Desarrollo (9 horas)

### Objetivo específico:
Desarrollar una aplicación completa con funcionalidad CRUD.

### Actividades:

#### 1. Modelos y base de datos (3 horas)
- **Demostrativa**: Creación de modelos y migraciones (1 hora)
- **Autónoma**: Los estudiantes crean modelos para su proyecto (2 horas)

#### 2. Vistas y plantillas (3 horas)
- **Demostrativa**: Creación de vistas y plantillas con herencia (1.5 horas)
- **Autónoma**: Los estudiantes implementan vistas para su proyecto (1.5 horas)

#### 3. Formularios y archivos estáticos (3 horas)
- **Demostrativa**: Formularios CRUD y estilos CSS (1.5 horas)
- **Autónoma**: Los estudiantes implementan formularios y estilos (1.5 horas)

---

## Momento 3: Cierre (3 horas)

### Objetivo específico:
Consolidar los conocimientos y presentar el proyecto final.

### Actividades:

#### 1. Integración y depuración (1.5 horas)
- **Demostrativa**: Resolución de problemas comunes (30 min)
- **Autónoma**: Depuración y mejora de proyectos (60 min)

#### 2. Presentación de proyectos (1 hora)
- Exposición breve de cada proyecto (5 min por estudiante)
- Retroalimentación grupal

#### 3. Evaluación y cierre (30 min)
- Cuestionario de evaluación de conocimientos
- Entrega de reconocimientos

---

## Proyecto Final: Sistema de Inventario de Productos

### Modelo Producto (models.py)
```python
from django.db import models

class Producto(models.Model):
    CATEGORIAS = [
        ('ELECT', 'Electrónicos'),
        ('ROPA', 'Ropa y Accesorios'),
        ('HOGAR', 'Hogar y Jardín'),
        ('DEPORT', 'Deportes'),
        ('OTROS', 'Otros'),
    ]
    
    nombre = models.CharField(max_length=100, verbose_name="Nombre del Producto")
    descripcion = models.TextField(verbose_name="Descripción")
    precio = models.DecimalField(max_digits=10, decimal_places=2, verbose_name="Precio")
    categoria = models.CharField(max_length=50, choices=CATEGORIAS, default='OTROS', verbose_name="Categoría")
    stock = models.IntegerField(verbose_name="Cantidad en Stock")
    fecha_creacion = models.DateTimeField(auto_now_add=True, verbose_name="Fecha de Creación")
    foto = models.ImageField(upload_to='productos/', blank=True, null=True, verbose_name="Imagen del Producto")
    
    def __str__(self):
        return f"{self.nombre} - ${self.precio}"
    
    class Meta:
        verbose_name = "Producto"
        verbose_name_plural = "Productos"
        ordering = ['-fecha_creacion']
```

### Características del proyecto:
1. CRUD completo para productos
2. Filtrado por categorías
3. Búsqueda de productos
4. Interfaz responsive con Bootstrap
5. Subida y visualización de imágenes

---

## Glosario de Términos

- **Django**: Framework de desarrollo web en Python
- **Modelo**: Representación de la estructura de base de datos
- **Vista**: Función que procesa peticiones y devuelve respuestas
- **Plantilla**: Archivo HTML con sintaxis especial de Django
- **URLconf**: Configuración de rutas URL
- **Migración**: Sistema de control de cambios en la base de datos
- **CRUD**: Create, Read, Update, Delete (Crear, Leer, Actualizar, Eliminar)
- **ORM**: Mapeo Objeto-Relacional (Object-Relational Mapping)
- **Middleware**: Componente que procesa peticiones/respuestas

---

## Recursos y Referencias

### Documentación oficial:
- [Django Documentation](https://docs.djangoproject.com/)
- [Python Documentation](https://docs.python.org/)
- [HTML Reference](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS)

### Tutoriales recomendados:
- [Django for Beginners](https://djangoforbeginners.com/)
- [MDN Web Docs - Django](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django)
- [Real Python Django Tutorials](https://realpython.com/tutorials/django/)

### Herramientas:
- [VS Code](https://code.visualstudio.com/)
- [DB Browser for SQLite](https://sqlitebrowser.org/)
- [Bootstrap](https://getbootstrap.com/) (para mejorar la interfaz)

### Bibliografía recomendada:
1. "Django for Beginners" - William S. Vincent
2. "Django 3 By Example" - Antonio Mele
3. "Two Scoops of Django" - Daniel Roy Greenfeld & Audrey Roy Greenfeld
4. "Python Crash Course" - Eric Matthes

---

## Evaluación del Curso

### Criterios de evaluación:
1. Funcionalidad completa del CRUD (30%)
2. Calidad del código y organización (25%)
3. Diseño de interfaz y experiencia de usuario (20%)
4. Presentación del proyecto (15%)
5. Participación en clase (10%)

### Rúbrica para el proyecto final:
- **Excelente (9-10)**: Proyecto completo, funcional, bien diseñado y con extras
- **Bueno (7-8)**: Proyecto completo y funcional con diseño aceptable
- **Satisfactorio (6)**: Proyecto funciona pero con detalles menores
- **Insuficiente (<6)**: Proyecto incompleto o no funcional

---

## Material Adicional para Instructores

### Solución de problemas comunes:
1. **Error de instalación**: Verificar versión de Python y PATH
2. **Error de migraciones**: Ejecutar `python manage.py makemigrations` y luego `migrate`
3. **Error de importación**: Verificar nombres de aplicaciones en settings.py
4. **Archivos estáticos no cargan**: Verificar configuración en settings.py y uso de `{% load static %}`

### Extensiones recomendadas para VS Code:
- Python
- Django
- HTML CSS Support
- Auto Rename Tag
- Bracket Pair Colorizer
- GitLens
- GitHub Copilot

### Consejos para la enseñanza:
1. Comenzar con ejemplos simples y aumentar complejidad gradualmente
2. Fomentar la experimentación y aprendizaje por descubrimiento
3. Proporcionar ejemplos de código bien comentados
4. Destacar las mejores prácticas desde el principio
5. Mostrar cómo debuggear errores comunes

¡Éxito en la implementación del curso!
