¡Excelente! Aquí tienes una guía completa y detallada para la Clase 2 de Django, diseñada para estudiantes de preparatoria en Windows. Cada paso está explicado para facilitar el aprendizaje y la comprensión.

---

### **Clase 2: Django con Python para Aplicaciones Web**

**¡Bienvenidos a la segunda clase de Django!** Hoy vamos a construir una aplicación web completa desde cero. Aprenderemos a manejar artistas y sus canciones, cubriendo todo el ciclo de una aplicación: Crear, Leer, Actualizar y Borrar datos (conocido como CRUD).

**Objetivos de hoy:**
*   Crear un proyecto y una aplicación en Django.
*   Definir modelos para nuestra base de datos.
*   Usar el panel de administración de Django.
*   Crear vistas y plantillas HTML para mostrar la información.
*   Implementar la funcionalidad CRUD completa con formularios.
*   Añadir estilos CSS para que nuestra aplicación se vea genial.

**Recomendaciones de Software:**
*   **Extensiones de VS Code:** Las que mencionaste son perfectas. Te recomiendo añadir **"SQLite"** por `alexcvzz`. Te permitirá ver y explorar tu base de datos directamente desde VS Code.
*   **Visor de Base de Datos:** La extensión de VS Code "SQLite" es la opción más sencilla y recomendada para empezar, ya que no necesitas instalar software adicional.

---

### **Procedimiento Paso a Paso**

#### **Parte 1: Configuración del Proyecto**

**1. Crear carpeta de trabajo “Curso_Django”**
*   **Explicación:** Es una buena práctica tener una carpeta principal para todos los proyectos del curso. Esto mantiene todo organizado.
*   **Acción:** Abre el Explorador de Archivos, ve a tus Documentos (o donde prefieras) y crea una nueva carpeta llamada `Curso_Django`.

**2. Crear subcarpeta “Artista” para el primer proyecto**
*   **Explicación:** Dentro de la carpeta del curso, cada proyecto tendrá su propia carpeta.
*   **Acción:** Dentro de `Curso_Django`, crea otra carpeta llamada `Artista`.

**3. Abrir VS Code desde Artista**
*   **Explicación:** Abrir VS Code directamente en la carpeta del proyecto facilita el acceso a todos los archivos y el uso de la terminal integrada.
*   **Acción:** Entra en la carpeta `Artista`. Haz clic derecho en un espacio vacío y selecciona **"Abrir con Code"**. Si no tienes esa opción, abre VS Code, ve a `Archivo > Abrir Carpeta...` y selecciona la carpeta `Artista`.

**4. Abrir nueva terminal de VS Code y verificar versiones**
*   **Explicación:** La terminal es nuestra herramienta para dar instrucciones a la computadora. Verificaremos que Python y su gestor de paquetes `pip` estén instalados correctamente.
*   **Acción:** En VS Code, ve al menú superior `Terminal > Nueva terminal`. Escribe los siguientes comandos, presionando Enter después de cada uno:
    ```bash
    python --version
    pip --version
    ```
    *Deberías ver las versiones instaladas, por ejemplo: `Python 3.11.4`.*

**5. Crear el entorno virtual `.venv`**
*   **Explicación:** Un entorno virtual es como una caja de herramientas limpia y separada para cada proyecto. Así, las "herramientas" (paquetes de Python) que instalemos para este proyecto no se mezclarán con las de otros.
*   **Acción:** En la terminal, ejecuta:
    ```bash
    python -m venv .venv
    ```
    *Verás que aparece una nueva carpeta `.venv` en tu proyecto.*

**6. Activar entorno virtual**
*   **Explicación:** Ahora que tenemos la caja de herramientas, debemos "abrirla" para empezar a usarla.
*   **Acción:** En la terminal de VS Code, ejecuta el siguiente comando. ¡Presta atención a las barras invertidas `\`!
    ```bash
    .\.venv\Scripts\activate
    ```
    *Notarás que al inicio de la línea de la terminal aparece `(.venv)`. ¡Esto significa que el entorno está activo!*

**7. Seleccionar intérprete de Python**
*   **Explicación:** Le diremos a VS Code que debe usar el Python que está dentro de nuestra "caja de herramientas" (`.venv`), no el Python general del sistema.
*   **Acción:**
    1.  Presiona `Ctrl+Shift+P` para abrir la paleta de comandos.
    2.  Escribe `Python: Select Interpreter`.
    3.  Elige la opción que tiene `('.venv': venv)` en la ruta. Suele ser la recomendada.

**8. Instalar Django y verificar versión**
*   **Explicación:** Ahora instalaremos Django, el framework que nos permitirá construir nuestra web, dentro de nuestro entorno activo.
*   **Acción:** En la terminal, ejecuta:
    ```bash
    pip install django
    ```
    Una vez instalado, verifica la versión:
    ```bash
    django-admin --version
    ```

**9. Crear el proyecto “backend_artista”**
*   **Explicación:** Un "proyecto" en Django es el contenedor principal de nuestra aplicación web. Contendrá la configuración general.
*   **Acción:** Ejecuta el siguiente comando. El punto `.` al final es muy importante, le dice a Django que cree el proyecto en la carpeta actual (`Artista`), evitando una carpeta extra innecesaria.
    ```bash
    django-admin startproject backend_artista .
    ```
    *Verás que aparecen el archivo `manage.py` y una carpeta `backend_artista`.*

**10. Ejecutar servidor integrado de Django**
*   **Explicación:** Django viene con un pequeño servidor web para que podamos probar nuestro proyecto mientras lo desarrollamos.
*   **Acción:**
    ```bash
    python manage.py runserver
    ```

**11. Para ver página web**
*   **Explicación:** El servidor nos dará una dirección web local para ver nuestra aplicación en el navegador.
*   **Acción:** Abre tu navegador (Chrome, Firefox, etc.) y ve a la dirección que aparece en la terminal: **http://127.0.0.1:8000/**. Verás la página de bienvenida de Django. ¡Felicidades, tu proyecto está vivo!

**12. Detener servidor Django**
*   **Explicación:** Para poder seguir ejecutando comandos, debemos detener el servidor.
*   **Acción:** Vuelve a la terminal de VS Code y presiona `Ctrl+C`.

---

#### **Parte 2: Base de Datos y Aplicación Principal**

**13. Realizar migración (makemigrations)**
*   **Explicación:** Este es el primer paso para preparar nuestra base de datos. En este momento, no hemos creado nuestros propios "modelos" de datos, así que este comando no hará mucho, pero es bueno conocerlo. Más adelante, cuando creemos el modelo `Artista`, este comando creará el "plano" de cómo se guardará un artista en la base de datos.
*   **Acción:**
    ```bash
    python manage.py makemigrations
    ```
    *Output esperado: `No changes detected`.*

**14. Hacer migraciones (migrate)**
*   **Explicación:** El comando `migrate` toma los "planos" creados por `makemigrations` y construye las tablas en la base de datos. Django ya viene con modelos predeterminados (para usuarios, permisos, etc.), y este comando los creará.
*   **Acción:**
    ```bash
    python manage.py migrate
    ```
    *Verás muchas líneas que dicen "Applying..." o "OK". Esto crea un archivo llamado `db.sqlite3` en tu proyecto. ¡Esa es tu base de datos!*

**15. Crear superusuario**
*   **Explicación:** Django incluye un panel de administración increíble. Para acceder, necesitamos un superusuario, que es el administrador principal del sitio.
*   **Acción:**
    ```bash
    python manage.py createsuperuser
    ```
    *El sistema te pedirá:*
    *   **Username:** Elige un nombre de usuario (ej: `admin`).
    *   **Email address:** Puedes poner tu email o dejarlo en blanco.
    *   **Password:** Escribe una contraseña. **¡No verás los caracteres mientras escribes, es normal!** Escríbela y presiona Enter. Te pedirá que la confirmes.

**16. Ejecutar servidor y acceder al panel de administración**
*   **Explicación:** Vamos a ver el panel de administración que acabamos de habilitar.
*   **Acción:**
    1.  Ejecuta el servidor: `python manage.py runserver`
    2.  En tu navegador, ve a **http://127.0.0.1:8000/admin/**.
    3.  Inicia sesión con el superusuario que acabas de crear. ¡Explora el panel!

**17. Detener el servidor**
*   **Acción:** Vuelve a la terminal y presiona `Ctrl+C`.

**18. Crear aplicación “app_artista”**
*   **Explicación:** Un proyecto de Django se compone de "aplicaciones". Piensa en el proyecto como la casa entera y una aplicación como una habitación (la cocina, el baño). Nuestra `app_artista` se encargará de todo lo relacionado con los artistas.
*   **Acción:**
    ```bash
    python manage.py startapp app_artista
    ```
    *Aparecerá una nueva carpeta `app_artista` con varios archivos dentro.*

---

#### **Parte 3: Creando la Lista de Artistas (El "Read" de CRUD)**

**19. En `app_artista` crear el archivo `urls.py`**
*   **Explicación:** Cada aplicación debe gestionar sus propias rutas (URLs). Este archivo le dirá a Django qué hacer cuando un usuario visite, por ejemplo, `/artistas/` o `/artistas/1/`.
*   **Acción:** Dentro de la carpeta `app_artista`, crea un nuevo archivo llamado `urls.py`.

**20. En `app_artista` en `views.py` crear función `listar_artistas`**
*   **Explicación:** Las vistas son funciones de Python que reciben una petición web y devuelven una respuesta (normalmente una página HTML). Por ahora, la dejaremos vacía, la completaremos más adelante.
*   **Acción:** Abre `app_artista/views.py` y añade:
    ```python
    from django.shortcuts import render

    # Esta es nuestra primera vista. La completaremos más tarde.
    def listar_artistas(request):
        pass # 'pass' es una instrucción que no hace nada, es un marcador de posición.
    ```

**21. En `backend_artista` de `settings.py` agregar la `app_artista`**
*   **Explicación:** Debemos registrar nuestra nueva aplicación en la configuración principal del proyecto para que Django sepa que existe.
*   **Acción:** Abre `backend_artista/settings.py`. Busca la lista `INSTALLED_APPS` y añade `'app_artista'` al final.
    ```python
    # backend_artista/settings.py

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'app_artista', # <-- AÑADIR ESTA LÍNEA
    ]
    ```

**22. En `backend_artista` incluir la ruta para `app_artista`**
*   **Explicación:** Ahora conectaremos las URLs principales del proyecto con las URLs de nuestra aplicación. Le diremos al proyecto: "Oye, si alguien visita una URL que empieza con `/`, mándale la petición a `app_artista` para que decida qué hacer".
*   **Acción:** Abre `backend_artista/urls.py` y modifícalo:
    ```python
    # backend_artista/urls.py
    from django.contrib import admin
    # Importamos 'include' para poder conectar con las urls de nuestra app
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        # Cuando alguien visite la raíz del sitio (''),
        # Django buscará más rutas en el archivo app_artista/urls.py
        path('', include('app_artista.urls')),
    ]
    ```

**23. En `app_artista/models.py` crear la clase “Artista”**
*   **Explicación:** Un modelo es la representación de una tabla en nuestra base de datos. La clase `Artista` será el "molde" para cada artista que guardemos, definiendo qué datos tiene (nombre, nacionalidad, foto).
*   **Acción:** Abre `app_artista/models.py` y escribe el siguiente código:
    ```python
    # app_artista/models.py
    from django.db import models

    # Esta clase define la estructura de un Artista en la base de datos.
    class Artista(models.Model):
        # Campo de texto con un máximo de 100 caracteres.
        nombre = models.CharField(max_length=100)
        # Otro campo de texto para la nacionalidad.
        nacionalidad = models.CharField(max_length=50)
        # Campo para subir imágenes. 'upload_to' especifica la subcarpeta
        # donde se guardarán las fotos dentro de nuestra carpeta de 'media'.
        foto_artista = models.ImageField(upload_to='img_artistas/')

        # El método __str__ define cómo se mostrará un objeto Artista
        # cuando lo veamos en el panel de admin o en la terminal.
        def __str__(self):
            return self.nombre

        # La clase Meta nos permite añadir configuraciones extra al modelo.
        # Aquí, le decimos a Django cómo llamar al modelo en plural.
        class Meta:
            verbose_name_plural = "Artistas"

    ```
    **¡Importante!** Para que `ImageField` funcione, necesitamos instalar una librería llamada `Pillow`. En la terminal (con el entorno activado), ejecuta:
    ```bash
    pip install Pillow
    ```

**24. Registrar el modelo “Artista” en `admin.py` de `app_artista`**
*   **Explicación:** Para poder agregar, editar y borrar artistas desde el panel de administración, debemos registrar nuestro modelo `Artista`.
*   **Acción:** Abre `app_artista/admin.py` y modifícalo:
    ```python
    # app_artista/admin.py
    from django.contrib import admin
    # Importamos nuestro modelo Artista desde el archivo models.py
    from .models import Artista

    # Le decimos a Django: "Muestra el modelo Artista en el panel de administración".
    admin.site.register(Artista)
    ```

**25. Crear carpeta “templates” dentro de `app_artista`**
*   **Explicación:** Django busca por defecto los archivos HTML (plantillas) dentro de una carpeta llamada `templates` en cada aplicación.
*   **Acción:** Dentro de `app_artista`, crea una nueva carpeta llamada `templates`.

**26. Crear el archivo “listar_artistas.html” dentro de `templates`**
*   **Explicación:** Este será el archivo HTML que mostrará la lista de todos nuestros artistas.
*   **Acción:** Dentro de la carpeta `templates`, crea un archivo llamado `listar_artistas.html`.

**27. Código de `listar_artistas.html`**
*   **Explicación:** Este código HTML utiliza el lenguaje de plantillas de Django. `{% for ... %}` es un bucle que recorrerá la lista de artistas que le pasemos desde la vista. `{{ artista.nombre }}` mostrará el nombre de cada artista.
*   **Acción:** Pega este código en `app_artista/templates/listar_artistas.html`:
    ```html
    <!-- app_artista/templates/listar_artistas.html -->
    <h2>Artistas <a href="">(+)</a></h2>
    <ul>
      <!-- Esto es un bucle de plantilla Django.
           Recorrerá cada 'artista' en la lista 'artistas' que le pasemos desde la vista. -->
      {% for artista in artistas %}
      <li>
        <!-- Muestra el atributo 'nombre' del objeto 'artista' actual. -->
        <a href="">{{ artista.nombre }}</a>
      </li>
      {% endfor %}
    </ul>
    ```

**28. En `app_artista/views.py` completar la función `listar_artistas`**
*   **Explicación:** Ahora vamos a darle vida a nuestra vista. Obtendremos todos los artistas de la base de datos y se los pasaremos a nuestra plantilla HTML para que los muestre.
*   **Acción:** Modifica `app_artista/views.py`:
    ```python
    # app_artista/views.py
    from django.shortcuts import render
    # Importamos el modelo Artista para poder consultarlo.
    from .models import Artista

    # Vista para mostrar la lista de todos los artistas.
    def listar_artistas(request):
        # 1. Obtenemos todos los objetos Artista de la base de datos.
        artistas = Artista.objects.all()

        # 2. Creamos un 'contexto', que es un diccionario para pasar datos a la plantilla.
        #    La clave 'artistas' será el nombre de la variable en el HTML.
        #    El valor es la lista de artistas que obtuvimos.
        contexto = {'artistas': artistas}

        # 3. Renderizamos (dibujamos) la plantilla HTML, pasándole la petición y el contexto.
        #    Django buscará 'listar_artistas.html' en la carpeta 'templates'.
        return render(request, 'listar_artistas.html', contexto)
    ```

**29. Hacer migraciones**
*   **Explicación:** Ahora que hemos creado el modelo `Artista`, debemos crear el "plano" de migración para él.
*   **Acción:**
    ```bash
    python manage.py makemigrations app_artista
    ```
    *Verás que se crea un nuevo archivo de migración en `app_artista/migrations/`.*

**30. Realizar migrate**
*   **Explicación:** Aplicamos el plano a la base de datos, creando la tabla para los artistas.
*   **Acción:**
    ```bash
    python manage.py migrate
    ```

**31. Ejecutar servidor y acceder al panel de administración**
*   **Acción:** `python manage.py runserver` y ve a **http://127.0.0.1:8000/admin/**.

**32. Agregar 3 artistas**
*   **Explicación:** Ahora verás una sección "Artistas" en el panel. Vamos a añadir datos para probar nuestra aplicación. Como aún no hemos creado el modelo `Cancion`, ignora esa parte de la instrucción por ahora.
*   **Acción:**
    1.  Haz clic en "Artistas".
    2.  Haz clic en "Add artista +".
    3.  Rellena el nombre, nacionalidad y sube una foto para 3 artistas diferentes. Guarda cada uno.

**33. Visitar el sitio para observar la lista de artistas**
*   **Explicación:** Todavía no hemos conectado nuestra vista a una URL. ¡Hagámoslo!
*   **Acción:**
    1.  Abre `app_artista/urls.py` y añade la ruta para nuestra vista `listar_artistas`:
        ```python
        # app_artista/urls.py
        from django.urls import path
        from . import views # Importamos el archivo views.py

        urlpatterns = [
            # Cuando el usuario visite la URL raíz (''), se ejecutará la vista 'listar_artistas'.
            # Le damos un nombre 'listar_artistas' para poder referenciarla fácilmente en las plantillas.
            path('', views.listar_artistas, name='listar_artistas'),
        ]
        ```
    2.  Asegúrate de que el servidor esté corriendo (`python manage.py runserver`).
    3.  Ve a tu navegador y visita **http://127.0.0.1:8000/**. ¡Deberías ver la lista de los artistas que agregaste!

**34. Inspeccionar página web**
*   **Acción:** Haz clic derecho en la página y selecciona "Inspeccionar". Explora la pestaña "Elements" para ver el HTML que Django ha generado.

**35. Utilizar visor de base de datos para SQLite**
*   **Acción:**
    1.  En VS Code, ve al panel de extensiones y busca e instala **"SQLite"** (de alexcvzz).
    2.  En el explorador de archivos de VS Code, haz clic derecho sobre `db.sqlite3` y selecciona **"Open Database"**.
    3.  En el panel de la izquierda, aparecerá un explorador de SQLite. Despliega tu base de datos y verás la tabla `app_artista_artista`. Haz clic en ella para ver los datos que has insertado.

**36. Detener servidor**
*   **Acción:** `Ctrl+C` en la terminal.

---

#### **Parte 4: Vista de Detalle y Enrutamiento Dinámico**

**37. Agregar la función “detalle_artista” en `views.py`**
*   **Explicación:** Esta nueva vista mostrará la información de un solo artista, identificado por su ID.
*   **Acción:** Abre `app_artista/views.py` y añade la nueva función:
    ```python
    # app_artista/views.py
    # ... (código existente) ...

    # --- NUEVO CÓDIGO ---
    # Vista para mostrar los detalles de un artista específico.
    # Recibe el 'request' y también el 'artista_id' que viene de la URL.
    def detalle_artista(request, artista_id):
        # Usamos Artista.objects.get() para buscar un único artista por su 'id'.
        artista = Artista.objects.get(id=artista_id)
        # Pasamos el artista encontrado a la plantilla 'detalle_artista.html'.
        return render(request, 'detalle_artista.html', {'artista': artista})
    ```

**38. Incluir el código de la nueva función**
*   Hecho en el paso anterior.

**39. Agregar la siguiente ruta en `urls.py` de `app_artista`**
*   **Explicación:** Esta nueva ruta es dinámica. `<int:artista_id>` es un patrón que le dice a Django que espere un número entero en la URL (como `/artista/1/` o `/artista/5/`) y que pase ese número a nuestra vista `detalle_artista` como el parámetro `artista_id`.
*   **Acción:** Actualiza `app_artista/urls.py`:
    ```python
    # app_artista/urls.py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.listar_artistas, name='listar_artistas'),
        # --- NUEVO CÓDIGO ---
        # Ruta para el detalle de un artista.
        # <int:artista_id> captura un número de la URL y lo pasa como 'artista_id' a la vista.
        path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    ]
    ```

**40. En `listar_artistas.html` actualizar el enlace**
*   **Explicación:** Ahora que tenemos una ruta para el detalle, vamos a hacer que cada nombre de artista en la lista sea un enlace a su propia página de detalles. Usamos la etiqueta `{% url %}` de Django, que es la forma correcta de construir URLs. Es mejor que escribir `/artista/1/` a mano, porque si cambiamos la ruta en `urls.py`, el enlace se actualizará automáticamente.
*   **Acción:** Modifica `app_artista/templates/listar_artistas.html`:
    ```html
    <!-- app_artista/templates/listar_artistas.html -->
    <h2>Artistas <a href="">(+)</a></h2>
    <ul>
      {% for artista in artistas %}
      <li>
        <!-- CAMBIO REALIZADO: El enlace ahora apunta a la URL 'detalle_artista',
             pasándole el 'id' del artista actual del bucle. -->
        <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
      </li>
      {% endfor %}
    </ul>
    ```

**41. En `templates` crear el archivo `detalle_artista.html`**
*   **Acción:** Dentro de `app_artista/templates/`, crea un nuevo archivo `detalle_artista.html`.

**42. Código de `detalle_artista.html`**
*   **Explicación:** Esta plantilla mostrará los detalles del artista que le pasamos desde la vista. Ignoraremos la parte de las canciones por ahora. Para que la imagen se muestre, necesitaremos configurar los archivos `media` más adelante.
*   **Acción:** Pega este código en el nuevo archivo:
    ```html
    <!-- app_artista/templates/detalle_artista.html -->
    <h2>{{ artista.nombre }} <a href="">(edit)</a></h2>
    <h4>{{ artista.nacionalidad }}</h4>

    <!-- La propiedad .url del campo de imagen nos da la ruta para mostrarla. -->
    <img src="{{ artista.foto_artista.url }}" alt="Foto de {{ artista.nombre }}" width="300" height="250" />

    <!-- Dejamos esto como marcador de posición para el futuro. -->
    <h3>Canciones <a href="">(+)</a></h3>
    <ul>
      <li>Canción 1</li>
      <li>Canción 2</li>
    </ul>
    ```

**43. Ejecutar servidor Django para ver detalles del artista**
*   **Explicación:** Antes de ejecutar, necesitamos configurar cómo Django maneja los archivos subidos por los usuarios (media files), como las fotos de los artistas.
*   **Acción:**
    1.  **Detén el servidor** (`Ctrl+C`).
    2.  Abre `backend_artista/settings.py` y añade estas líneas al final del archivo:
        ```python
        # backend_artista/settings.py
        # ... al final del archivo ...

        # URL base para servir archivos de media subidos por el usuario.
        MEDIA_URL = '/media/'
        # Ruta en el disco duro donde se guardarán los archivos de media.
        MEDIA_ROOT = BASE_DIR / 'media'
        ```
    3.  Abre `backend_artista/urls.py` y modifícalo para que el servidor de desarrollo pueda mostrar las imágenes:
        ```python
        # backend_artista/urls.py
        from django.contrib import admin
        from django.urls import path, include
        # Importaciones necesarias para servir archivos estáticos y de media en desarrollo
        from django.conf import settings
        from django.conf.urls.static import static

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('', include('app_artista.urls')),
        ]

        # Esta línea es para que el servidor de desarrollo de Django
        # pueda encontrar y mostrar los archivos que subimos (fotos de artistas).
        # ¡Esto NO se usa en un servidor real (producción)!
        if settings.DEBUG:
            urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
        ```
    4.  Ahora sí, ejecuta el servidor: `python manage.py runserver`.
    5.  Ve a **http://127.0.0.1:8000/**. Haz clic en el nombre de un artista. ¡Deberías ver su página de detalle con la foto!

**44. Parar servidor**
*   **Acción:** `Ctrl+C` en la terminal.

---

#### **Parte 5: Estilos, Plantillas Base y CRUD Completo**

**45. Crear carpetas `static/css`**
*   **Explicación:** Los archivos estáticos (CSS, JavaScript, imágenes del diseño) se organizan en una carpeta `static` dentro de cada aplicación.
*   **Acción:** Dentro de `app_artista`, crea una carpeta `static`. Dentro de `static`, crea otra carpeta `css`. La ruta será `app_artista/static/css/`.

**46. Crear el archivo `styles.css`**
*   **Acción:** Dentro de `app_artista/static/css/`, crea un archivo llamado `styles.css`.

**47. Código para `styles.css`**
*   **Explicación:** Añadiremos algunos estilos básicos para que la web se vea más limpia y profesional.
*   **Acción:** Pega este código en `styles.css`:
    ```css
    /* app_artista/static/css/styles.css */

    /* Estilo general del cuerpo de la página */
    body {
        font-family: Arial, sans-serif;
        background-color: #f4f4f4;
        color: #333;
        margin: 0;
        padding: 20px;
    }

    /* Contenedor principal para centrar el contenido */
    .container {
        max-width: 800px;
        margin: auto;
        background: #fff;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    h1, h2, h3 {
        color: #0056b3;
    }

    /* Estilo de la barra de navegación */
    nav {
        background: #0056b3;
        padding: 15px;
        text-align: center;
        border-radius: 5px;
        margin-bottom: 20px;
    }

    nav a {
        color: white;
        text-decoration: none;
        margin: 0 15px;
        font-weight: bold;
    }

    nav a:hover {
        text-decoration: underline;
    }

    /* Estilos para listas y enlaces */
    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        background: #e9ecef;
        margin-bottom: 5px;
        padding: 10px;
        border-radius: 4px;
    }

    a {
        text-decoration: none;
        color: #007bff;
    }

    a:hover {
        color: #0056b3;
    }

    /* Estilo para los botones */
    .btn {
        display: inline-block;
        padding: 10px 15px;
        color: #fff;
        background-color: #007bff;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        text-align: center;
        margin: 5px 2px;
    }

    .btn-danger {
        background-color: #dc3545;
    }
    .btn-secondary {
        background-color: #6c757d;
    }

    /* Estilo para los formularios */
    form p {
        margin-bottom: 15px;
    }

    form label {
        display: block;
        margin-bottom: 5px;
    }

    form input[type="text"],
    form input[type="file"] {
        width: 100%;
        padding: 8px;
        box-sizing: border-box;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
    ```

**48. Dentro de `templates` crear el archivo `base.html`**
*   **Explicación:** La plantilla base es un "esqueleto" de HTML. Contiene las partes comunes de todas las páginas (como el encabezado, la navegación, el pie de página). Las otras plantillas "heredarán" de esta y solo rellenarán las partes específicas.
*   **Acción:** Dentro de `app_artista/templates/`, crea `base.html`.

**49. Código de `base.html`**
*   **Explicación:**
    *   `{% load static %}`: Carga las herramientas de Django para manejar archivos estáticos.
    *   `<link ... href="{% static 'css/styles.css' %}">`: Carga nuestra hoja de estilos de forma segura.
    *   `{% block contenido %}{% endblock %}`: Define un "bloque" que las plantillas hijas podrán rellenar con su contenido específico.
*   **Acción:** Pega este código en `base.html`:
    ```html
    <!-- app_artista/templates/base.html -->
    {% load static %} <!-- Carga las etiquetas de archivos estáticos de Django -->
    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Melodías</title>
        <!-- Enlaza nuestra hoja de estilos CSS usando la etiqueta static -->
        <link rel="stylesheet" href="{% static 'css/styles.css' %}" />
    </head>
    <body>
        <div class="container">
            <h1>Melodías</h1>
            <nav>
                <!-- Más adelante crearemos la app de canciones -->
                <a href="">Canciones</a>
                <!-- El enlace a la raíz del sitio ('/') nos llevará a la lista de artistas -->
                <a href="{% url 'listar_artistas' %}">Artistas</a>
            </nav>

            <!-- Este es un bloque de contenido.
                 Las plantillas que hereden de esta base pondrán su contenido aquí. -->
            {% block contenido %}
            {% endblock %}
        </div>
    </body>
    </html>
    ```

**50. y 51. Actualizar `listar_artistas.html` para usar la plantilla base**
*   **Explicación:** Vamos a decirle a `listar_artistas.html` que use `base.html` como su esqueleto.
*   **Acción:** Modifica `listar_artistas.html` para que quede así:
    ```html
    <!-- app_artista/templates/listar_artistas.html -->

    <!-- NUEVO: Le decimos a Django que esta plantilla extiende (usa como base) a base.html -->
    {% extends 'base.html' %}

    <!-- NUEVO: Todo el contenido específico de esta página va dentro del bloque 'contenido' -->
    {% block contenido %}

    <h2>Artistas <a href="{% url 'crear_artista' %}" class="btn">(+) Crear Nuevo</a></h2>
    <ul>
      {% for artista in artistas %}
      <li>
        <a href="{% url 'detalle_artista' artista.id %}">{{ artista.nombre }}</a>
      </li>
      {% endfor %}
    </ul>

    {% endblock %} <!-- Fin del bloque de contenido -->
    ```
    También actualiza `detalle_artista.html` de la misma forma:
    ```html
    <!-- app_artista/templates/detalle_artista.html -->
    {% extends 'base.html' %}

    {% block contenido %}

    <h2>{{ artista.nombre }}</h2>
    <h4>{{ artista.nacionalidad }}</h4>

    {% if artista.foto_artista %}
        <img src="{{ artista.foto_artista.url }}" alt="Foto de {{ artista.nombre }}" width="300" height="250" />
    {% endif %}

    <hr>
    <a href="{% url 'listar_artistas' %}" class="btn btn-secondary">Volver a la lista</a>

    {% endblock %}
    ```

**52. En `setting.py` indicar cambios para archivos estáticos**
*   **Explicación:** Ya hicimos los cambios para `media` (imágenes subidas). Ahora, aunque Django encuentra los archivos `static` por defecto, es buena práctica definir `STATICFILES_DIRS` si tuviéramos una carpeta estática a nivel de proyecto. Por ahora, nuestra configuración es suficiente, pero aquí está el código completo para `static` y `media`.
*   **Acción:** Verifica que al final de `backend_artista/settings.py` tengas estas líneas:
    ```python
    # backend_artista/settings.py

    # ... al final del archivo ...

    # --- Configuración de Archivos Estáticos (CSS, JS, etc.) ---
    STATIC_URL = 'static/'
    # Opcional para este proyecto, pero buena práctica:
    # STATICFILES_DIRS = [BASE_DIR / 'static']

    # --- Configuración de Archivos Media (subidos por el usuario) ---
    MEDIA_URL = '/media/'
    MEDIA_ROOT = BASE_DIR / 'media'
    ```
    Y en `backend_artista/urls.py`, verifica que tienes la configuración para servir los archivos media en desarrollo.

**53. Crear `forms.py` y las clases de formulario**
*   **Explicación:** Los formularios de Django nos facilitan enormemente la creación, validación y procesamiento de formularios HTML. Crearemos un formulario basado en nuestro modelo `Artista`.
*   **Acción:**
    1.  Dentro de `app_artista`, crea un nuevo archivo llamado `forms.py`.
    2.  Añade el siguiente código:
        ```python
        # app_artista/forms.py
        from django import forms
        from .models import Artista

        # Este es un ModelForm. Django lo crea automáticamente
        # a partir del modelo que le especifiquemos.
        class ArtistaForm(forms.ModelForm):
            class Meta:
                # Le decimos al formulario en qué modelo debe basarse.
                model = Artista
                # Especificamos los campos del modelo que queremos incluir en el formulario.
                fields = ['nombre', 'nacionalidad', 'foto_artista']
        ```

**54. En `views.py` incluir las funciones `crear_artista`, `editar_artista` y `borrar_artista`**
*   **Explicación:** Estas son las vistas que completarán nuestra funcionalidad CRUD (Crear, Actualizar, Borrar).
*   **Acción:** Añade estas nuevas funciones a `app_artista/views.py`:
    ```python
    # app_artista/views.py
    from django.shortcuts import render, redirect, get_object_or_404
    from .models import Artista
    # Importamos el formulario que acabamos de crear.
    from .forms import ArtistaForm

    # ... (listar_artistas y detalle_artista ya existen) ...

    # --- VISTA PARA CREAR ---
    def crear_artista(request):
        # Si el método es POST, significa que el usuario ha enviado el formulario.
        if request.method == 'POST':
            # Creamos una instancia del formulario con los datos enviados y los archivos.
            form = ArtistaForm(request.POST, request.FILES)
            # is_valid() comprueba si los datos son correctos.
            if form.is_valid():
                form.save() # Guarda el nuevo artista en la base de datos.
                return redirect('listar_artistas') # Redirige al usuario a la lista.
        else:
            # Si el método es GET, creamos un formulario vacío para mostrarlo.
            form = ArtistaForm()
        # Renderizamos la plantilla del formulario, pasándole el formulario.
        return render(request, 'formulario_artista.html', {'form': form})

    # --- VISTA PARA EDITAR ---
    def editar_artista(request, artista_id):
        # Obtenemos el artista que se va a editar, o mostramos un error 404 si no existe.
        artista = get_object_or_404(Artista, id=artista_id)
        if request.method == 'POST':
            # Creamos el formulario con los datos enviados y le decimos que es para editar
            # la instancia 'artista' que obtuvimos.
            form = ArtistaForm(request.POST, request.FILES, instance=artista)
            if form.is_valid():
                form.save() # Guarda los cambios.
                return redirect('listar_artistas')
        else:
            # Si es GET, creamos el formulario con los datos del artista existente.
            form = ArtistaForm(instance=artista)
        return render(request, 'formulario_artista.html', {'form': form})

    # --- VISTA PARA BORRAR ---
    def borrar_artista(request, artista_id):
        artista = get_object_or_404(Artista, id=artista_id)
        if request.method == 'POST':
            artista.delete() # Borra el artista de la base de datos.
            return redirect('listar_artistas')
        # Si es GET, mostramos una página de confirmación.
        return render(request, 'confirmar_borrado.html', {'artista': artista})
    ```

**55. Actualizar el archivo `urls.py` de la `app_artista`**
*   **Explicación:** Necesitamos añadir las rutas para nuestras nuevas vistas de crear, editar y borrar.
*   **Acción:** Modifica `app_artista/urls.py`:
    ```python
    # app_artista/urls.py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.listar_artistas, name='listar_artistas'),
        path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
        # --- NUEVAS RUTAS ---
        # Ruta para mostrar el formulario de creación.
        path('artista/crear/', views.crear_artista, name='crear_artista'),
        # Ruta para editar un artista existente.
        path('artista/<int:artista_id>/editar/', views.editar_artista, name='editar_artista'),
        # Ruta para borrar un artista.
        path('artista/<int:artista_id>/borrar/', views.borrar_artista, name='borrar_artista'),
    ]
    ```

**56. En `templates` crear el archivo `formulario_artista.html`**
*   **Explicación:** Esta plantilla servirá tanto para crear como para editar artistas. Mostrará el formulario que le pasemos desde la vista.
*   **Acción:** Crea el archivo `app_artista/templates/formulario_artista.html` y pega este código:
    ```html
    <!-- app_artista/templates/formulario_artista.html -->
    {% extends 'base.html' %}

    {% block contenido %}
      <h2>Formulario de Artista</h2>
      <!-- El 'enctype' es NECESARIO para que se puedan subir archivos (la foto). -->
      <form method="post" enctype="multipart/form-data">
        <!-- {% csrf_token %} es una medida de seguridad OBLIGATORIA en los formularios de Django. -->
        {% csrf_token %}

        <!-- {{ form.as_p }} renderiza cada campo del formulario como un párrafo <p>. -->
        {{ form.as_p }}

        <button type="submit" class="btn">Guardar</button>
        <a href="{% url 'listar_artistas' %}" class="btn btn-secondary">Cancelar</a>
      </form>
    {% endblock %}
    ```
    Y también crea la plantilla de confirmación de borrado, `confirmar_borrado.html`:
    ```html
    <!-- app_artista/templates/confirmar_borrado.html -->
    {% extends 'base.html' %}

    {% block contenido %}
        <h2>Confirmar Borrado</h2>
        <p>¿Estás seguro de que quieres borrar al artista "{{ artista.nombre }}"?</p>

        <form method="post">
            {% csrf_token %}
            <button type="submit" class="btn btn-danger">Sí, borrar</button>
            <a href="{% url 'detalle_artista' artista.id %}" class="btn btn-secondary">Cancelar</a>
        </form>
    {% endblock %}
    ```

**57. En el caso de `editar_artista` incluir la imagen con posibilidad de cambiarla.**
*   ¡Ya está hecho! Nuestro `ArtistaForm` incluye el campo `foto_artista`, y la vista `editar_artista` ya maneja `request.FILES`. El formulario mostrará el archivo actual y permitirá subir uno nuevo para reemplazarlo.

**58. Mostrar el código completo y actualizado de `detalle_artista.html`**
*   **Explicación:** Añadiremos los botones de Editar y Borrar a la página de detalle del artista.
*   **Acción:** Actualiza `app_artista/templates/detalle_artista.html`:
    ```html
    <!-- app_artista/templates/detalle_artista.html -->
    {% extends 'base.html' %}

    {% block contenido %}

    <h2>{{ artista.nombre }}</h2>
    <h4>{{ artista.nacionalidad }}</h4>

    {% if artista.foto_artista %}
        <img src="{{ artista.foto_artista.url }}" alt="Foto de {{ artista.nombre }}" width="300" height="250" />
    {% endif %}

    <hr>
    <!-- --- NUEVOS CAMBIOS: BOTONES DE ACCIÓN --- -->
    <a href="{% url 'editar_artista' artista.id %}" class="btn">Editar Artista</a>
    <a href="{% url 'borrar_artista' artista.id %}" class="btn btn-danger">Borrar Artista</a>
    <a href="{% url 'listar_artistas' %}" class="btn btn-secondary">Volver a la lista</a>

    {% endblock %}
    ```

**59. Ejecutar servidor para ver proyecto funcionando**
*   **Acción:** En la terminal, ejecuta `python manage.py runserver`.
*   **Prueba todo:**
    1.  Ve a **http://127.0.0.1:8000/**.
    2.  Haz clic en el botón **(+) Crear Nuevo**.
    3.  Crea un nuevo artista y sube su foto.
    4.  Vuelve a la lista y haz clic en el artista que acabas de crear.
    5.  En la página de detalle, haz clic en **Editar Artista**. Cambia su nacionalidad y guarda.
    6.  Vuelve a la página de detalle y haz clic en **Borrar Artista**. Confirma el borrado.

**60. Comentarios y explicaciones**
*   Todos los bloques de código en esta guía han sido comentados para explicar qué hace cada parte importante.

**61. Estructura de carpetas y archivos del proyecto**
*   Así es como debería verse tu proyecto al final:

```
Artista/
├── .venv/                      <-- Carpeta del entorno virtual
├── app_artista/
│   ├── migrations/             <-- Archivos de migración de la base de datos
│   │   ├── 0001_initial.py
│   │   └── __init__.py
│   ├── static/
│   │   └── css/
│   │       └── styles.css      <-- Nuestros estilos
│   ├── templates/
│   │   ├── base.html           <-- Plantilla base
│   │   ├── confirmar_borrado.html
│   │   ├── detalle_artista.html
│   │   ├── formulario_artista.html
│   │   └── listar_artistas.html
│   ├── __init__.py
│   ├── admin.py                <-- Registro de modelos para el admin
│   ├── apps.py
│   ├── forms.py                <-- Nuestros formularios Django
│   ├── models.py               <-- Nuestras clases de modelo (Artista)
│   ├── tests.py
│   ├── urls.py                 <-- Rutas específicas de la app
│   └── views.py                <-- Lógica de la aplicación (funciones/vistas)
├── backend_artista/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py             <-- Configuración principal del proyecto
│   ├── urls.py                 <-- Rutas principales del proyecto
│   └── wsgi.py
├── media/                      <-- Carpeta donde se guardan las fotos
│   └── img_artistas/
│       └── (aquí irán las fotos que subas)
├── db.sqlite3                  <-- Tu base de datos
└── manage.py                   <-- Utilidad de comandos de Django
```

**62. Proyecto totalmente funcional y atractivo**
*   ¡Lo lograste! Ahora tienes una aplicación web CRUD completamente funcional construida con Django. Puedes listar, ver, crear, editar y borrar artistas, todo con una apariencia limpia y organizada gracias a los estilos CSS y las plantillas base.

---

**¡Excelente trabajo!** Has cubierto los conceptos más importantes de Django en un solo proyecto. A partir de aquí, puedes experimentar añadiendo más modelos (como `Cancion`), mejorando los estilos o añadiendo nuevas funcionalidades. ¡El límite es tu imaginación
